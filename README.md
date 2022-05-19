# U4Summative
A place to store and turn in the unit 4 summative for Java

import java.awt.BorderLayout;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.Image;
import java.awt.event.ActionEvent;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;
import javax.swing.JButton;
import javax.swing.JFileChooser;

import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JScrollPane;

public class ImageFilters extends JPanel {

    static int WINDOW_WIDTH = 900;
    static int WINDOW_HEIGHT = 600;
    JButton LoadImageButton = new JButton("Load Image");
    JButton ResetButton = new JButton("Reset");
    JButton InvertButton = new JButton("Invert Colors");
    JButton GrayScaleButton = new JButton("Gray Scale");
    JButton StepColorsButton = new JButton("Step Colors");
    JButton EmbossButton = new JButton("Emboss Image");
    JButton BlurButton = new JButton("Blur Image");

    JFrame frame = new JFrame();
    BufferedImage image_pixels;
    
    JPanel buttons = new JPanel();
    static Image selected_image = null;
    File selected_file = null;
    
    public void InvertColors() {
        
      for (int i = 0; i < image_pixels.getHeight (); i++) 
      { 
        for (int j = 0; j < image_pixels.getWidth (); j++)
        {
          Pixel p = getPixel(j,i, image_pixels);
          int redValue = 255 - p.getRedValue();
          int greenValue = 255 - p.getGreenValue();
          int blueValue = 255 - p.getBlueValue();
          p.setRGB(redValue, greenValue, blueValue);
        }
          
      }
      
        
    }

    public void ConvertToGrayScale() {
        
    
    }

    public void StepColors() {
        
        // YOUR CODE HERE 
        
    }
    
    public void EmbossImage() {        
        // in case you'd like to reference the original image without altering it
        // use this image_copy to look at values and image_pixels to set the values
        BufferedImage image_copy = getImageCopy();

        // YOUR CODE HERE
        
    }
    
    public void BlurImage()
    {
        // THIS ONE IS EXTRA CREDIT
        // in case you'd like to reference the original image without altering it
        // use this image_copy to look at values and image_pixels to set the values
        BufferedImage image_copy = getImageCopy();
        
        // YOUR CODE HERE
        
    }

    @Override
    public void paintComponent(Graphics g) {
        
        if (selected_image != null)
        {
            g.drawImage(selected_image, 0, 0, this);            
        }
    }

    public static void main(String[] args) {
        final ImageFilters graphic = new ImageFilters();
        graphic.frame = new JFrame();
        JScrollPane scroll = new JScrollPane(graphic,
            JScrollPane.VERTICAL_SCROLLBAR_ALWAYS, 
            JScrollPane.HORIZONTAL_SCROLLBAR_AS_NEEDED);
        graphic.frame.getContentPane().add(scroll);

        graphic.frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        graphic.frame.setSize(WINDOW_WIDTH, WINDOW_HEIGHT + 100);
        graphic.LoadImageButton.addActionListener((ActionEvent e) -> {
            graphic.LoadImage();
            graphic.repaint();
        });
        graphic.ResetButton.addActionListener((ActionEvent e) -> {
            graphic.ResetImage();
            graphic.repaint();
        });
        graphic.InvertButton.addActionListener((ActionEvent e) -> {
            graphic.image_pixels = toBufferedImage(selected_image);
            graphic.ResetButton.setEnabled(true);
            graphic.InvertColors();
            graphic.repaint();
        });
        graphic.GrayScaleButton.addActionListener((ActionEvent e) -> {
            graphic.image_pixels = toBufferedImage(selected_image);
            graphic.ResetButton.setEnabled(true);
            graphic.ConvertToGrayScale();
            graphic.repaint();
        });
        graphic.StepColorsButton.addActionListener((ActionEvent e) -> {
            graphic.image_pixels = toBufferedImage(selected_image);
            graphic.ResetButton.setEnabled(true);
            graphic.StepColors();
            graphic.repaint();
        });
        graphic.EmbossButton.addActionListener((ActionEvent e) -> {
            graphic.image_pixels = toBufferedImage(selected_image);
            graphic.ResetButton.setEnabled(true);
            graphic.EmbossImage();
            graphic.repaint();
        });
        graphic.BlurButton.addActionListener((ActionEvent e) -> {
            graphic.image_pixels = toBufferedImage(selected_image);
            graphic.ResetButton.setEnabled(true);
            graphic.BlurImage();
            graphic.repaint();
        });
        graphic.buttons.add(graphic.LoadImageButton);
        graphic.buttons.add(graphic.ResetButton);
        graphic.buttons.add(graphic.InvertButton);
        graphic.buttons.add(graphic.GrayScaleButton);
        graphic.buttons.add(graphic.StepColorsButton);
        graphic.buttons.add(graphic.EmbossButton);
        graphic.buttons.add(graphic.BlurButton);
        
        graphic.ResetButton.setEnabled(false);
        graphic.InvertButton.setEnabled(false);
        graphic.GrayScaleButton.setEnabled(false);
        graphic.StepColorsButton.setEnabled(false);
        graphic.EmbossButton.setEnabled(false);
        graphic.BlurButton.setEnabled(false);
        
        
        graphic.frame.add(graphic.buttons, BorderLayout.SOUTH);
        graphic.revalidate();
        graphic.repaint();
        graphic.frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        graphic.frame.setVisible(true);
    }

    private void LoadImage() {
        JFileChooser fc = new JFileChooser();
        fc.showOpenDialog(ImageFilters.this);
       
        {
            if (fc.getSelectedFile() == null)
            {
                return;
            }
            selected_file = fc.getSelectedFile();
            selected_image = ImageIO.read(selected_file);
            this.setPreferredSize(new Dimension(selected_image.getWidth(null),selected_image.getHeight(null)));
            InvertButton.setEnabled(true);
            GrayScaleButton.setEnabled(true);
            StepColorsButton.setEnabled(true);
            EmbossButton.setEnabled(true);
            BlurButton.setEnabled(true);
            ResetButton.setEnabled(false);
            revalidate();
            repaint();
        }
        catch (IOException e)
        {
            System.out.println("There was an error with the file that was selected");
        }
    }
    
    private void ResetImage() {
        try
        {
            selected_image = ImageIO.read(selected_file);
            this.setPreferredSize(new Dimension(selected_image.getWidth(null),selected_image.getHeight(null)));
            ResetButton.setEnabled(false);
            revalidate();
            repaint();
        }
        catch (IOException e)
        {
            System.out.println("There was an error with the file that was selected");
        }
    }
    
    static BufferedImage getImageCopy() {
        
        Image clone = selected_image.getScaledInstance(selected_image.getWidth(null), -1, Image.SCALE_DEFAULT);
        
        return toBufferedImage(clone);
    }
    
    public static BufferedImage toBufferedImage(Image img)
    {
        if (img instanceof BufferedImage)
        {
            return (BufferedImage) img;
        }

        // Create a buffered image with transparency
        BufferedImage bimage = new BufferedImage(img.getWidth(null), img.getHeight(null), BufferedImage.TYPE_INT_ARGB);

        // Draw the image on to the buffered image
        Graphics2D bGr = bimage.createGraphics();
        bGr.drawImage(img, 0, 0, null);
        bGr.dispose();

        // Return the buffered image
        return bimage;
    }

    private Pixel getPixel(int column, int row, BufferedImage image) {
        Color currentPixelColor = new Color(image.getRGB(column, row));        
        int red = currentPixelColor.getRed();
        int green = currentPixelColor.getGreen();
        int blue = currentPixelColor.getBlue();
        return new Pixel(red,green,blue,column,row);
    }
    
    class Pixel {
        
        final private int red;
        final private int green;
        final private int blue;
        final private int column;
        final private int row;
        
        Pixel (int r, int g, int b, int column, int row)
        {
            red = r;
            green = g;
            blue = b;
            this.column = column;
            this.row = row;
        }
        
        public int getRedValue()
        {
            return red;
        }
        
        public int getGreenValue()
        {
            return green;
        }
        
        public int getBlueValue()
        {
            return blue;
        }
        
        public void setRGB(int r, int g, int b)
        {
            Color c = new Color(r,g,b);
            image_pixels.setRGB(column, row, c.getRGB());
        }
        
    }
    
}
