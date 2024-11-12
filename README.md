import java.util.Scanner;
import java.io.PrintWriter;
import java.io.IOException;
import java.io.FileInputStream;



public class ImageModify {
    public static int width;
    public static int height;
    
    enum Modification {
        NEGATE, QUANTIZE, GREYSCALE, FLIP
    }
    
    /**
     * Main method to modify an image based on user input.
     * @param args Command-line arguments (not used).
     * @throws IOException If an I/O error occurs.
     */
    public static void main(String[] args) throws IOException {
        Scanner scnr = new Scanner(System.in);
        System.out.print("Input file: ");
        String filename = scnr.nextLine();
        System.out.print("Output file: ");
        String outputFile = scnr.nextLine();
        System.out.println("Select modification: ");
        System.out.println("\t(a) negate");
        System.out.println("\t(b) quantize");
        System.out.println("\t(c) gray Scale");
        System.out.println("\t(d) flip horizontal");
        char s = scnr.nextLine().charAt(0);

        FileInputStream inStream = new FileInputStream(filename);
        PrintWriter outStream = new PrintWriter(outputFile);

        Scanner inScan = new Scanner(inStream);
        processHeader(inScan, outStream);
        if(s=='a'){
            processBody(inScan, outStream, Modification.NEGATE);
        }else if(s=='b'){
            processBody(inScan, outStream, Modification.QUANTIZE);
        }else if(s=='c'){
            processBody(inScan, outStream, Modification.GREYSCALE);
        }else if(s=='d'){   
            processBody(inScan, outStream, Modification.FLIP);
        }
        System.out.println(filename + " --> " + outputFile);
        System.out.println("Complete, closing files.");


        inScan.close();
        outStream.close();
        scnr.close();
    }
    
    /**
     * Processes the header of the image.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     */
    public static void processHeader(Scanner inScan, PrintWriter outStream){
        String style = inScan.nextLine();
        width = inScan.nextInt();
        height = inScan.nextInt();
        int color = inScan.nextInt();
        outStream.println(style);
        outStream.println(width + " " + height);
        outStream.println(color);
    }
    
    /**
     * Processes the body of the image based on the selected modification.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     * @param mod Character representing the selected modification.
     */
    private static void processBody(Scanner inScan, PrintWriter outStream, Modification mod){
        if(mod == Modification.NEGATE){
            NEGATE(inScan, outStream);
        }else if(mod == Modification.QUANTIZE){
            QUANTIZE(inScan, outStream);
        }else if(mod == Modification.GREYSCALE){
            GREYSCALE(inScan, outStream);
        }else if(mod == Modification.FLIP){
            FLIP(inScan, outStream);
        }else{
            System.out.println("Choose the correct one ");
        }
    }
    
    /**
     * Negates the colors of the image.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     */
    public static void NEGATE(Scanner inScan, PrintWriter outStream){
        for(int i = 0; i < height; i++){
            for(int j = 0; j < width; j++){
                int red = inScan.nextInt();
                int green = inScan.nextInt();
                int blue = inScan.nextInt();
                int totalR = Math.abs(red - 255);
                int totalG = Math.abs(green - 255);
                int totalB = Math.abs(blue - 255);
                outStream.print(totalR + " " + totalG + " " + totalB + " ");
            }
            outStream.println();
        }
    }
    
    /**
     * Quantizes the colors of the image.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     */
    public static void QUANTIZE(Scanner inScan, PrintWriter outStream){
        for(int i = 0; i<height; i++){
            for(int j = 0; j<width; j++){
                int red = inScan.nextInt();
                int green = inScan.nextInt();
                int blue = inScan.nextInt();
                if(red>127){
                    red=255;
                }else{
                    red = 0;
                }
                if(green>127){
                    green=255;
                }else{
                    green = 0;
                }
                if(blue>127){
                    blue=255;
                }else{
                    blue = 0;
                }
                outStream.print(red +" " + green + " " + blue +" ");
            }
            outStream.println();
        }
    }
    
    /**
     * Converts the image to grayscale.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     */
    public static void GREYSCALE(Scanner inScan, PrintWriter outStream){
        for(int i = 0; i < height; i++){
            for(int j = 0; j < width; j++){
                int red = inScan.nextInt();
                int green = inScan.nextInt();
                int blue = inScan.nextInt();
                int total = (red + green + blue) / 3;
                outStream.print(total + " " + total + " " + total + " ");
            }
            outStream.println();
        }
    }
    
    /**
     * Flips the image horizontally.
     * @param inScan Scanner for reading input file.
     * @param outStream PrintWriter for writing to output file.
     */
    public static void FLIP(Scanner inScan, PrintWriter outStream){
        int[] reds = new int[width];
        int[] greens = new int[width];
        int[] blues = new int[width];
        
        for(int i = 0; i < height; i++){
            for(int j = 0; j < width; j++){
                reds[j] = inScan.nextInt();
                greens[j] = inScan.nextInt();
                blues[j] = inScan.nextInt();
            }
            
            for(int j = width - 1; j >= 0; j--){
                outStream.print(reds[j] + " " + greens[j] + " " + blues[j] + " ");
            }
            outStream.println(); 
        }
    }
}
