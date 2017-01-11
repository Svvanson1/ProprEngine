package proprgame;

import java.awt.BorderLayout;
import java.awt.Canvas;
import java.awt.Color;
import java.awt.Dimension;
import java.awt.Graphics;
import java.awt.image.BufferStrategy;
import java.awt.image.BufferedImage;
import java.awt.image.DataBufferInt;

import javax.swing.JFrame;

public class Game extends Canvas implements Runnable {

	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;
	
	public static final int WIDTH = 160; //set width of game
	public static final int HEIGHT = WIDTH / 12 * 9; //set height of game
	public static final int SCALE = 3; //allows you to scale window
	public static final String NAME = "ProprGame"; //set game name
	
	private JFrame frame; //includes JFrame within package, allows variable change at a later date
	
	public boolean running = false;//boolean to set running of the game
	public int tickCount = 0;//count of total ticks
	
	private BufferedImage image = new BufferedImage(WIDTH, HEIGHT, BufferedImage.TYPE_INT_RGB); //creates image to be overlayed
	private int[] pixels = ((DataBufferInt) image.getRaster().getDataBuffer()).getData(); //pixels variable to update the image.
	
	
	public Game() { //constructor of game
		setMinimumSize(new Dimension(WIDTH * SCALE, HEIGHT * SCALE)); //set minimum size of canvas
		setMaximumSize(new Dimension(WIDTH * SCALE, HEIGHT * SCALE)); //set maximum size of canvas
		setPreferredSize(new Dimension(WIDTH * SCALE, HEIGHT * SCALE)); //set preferred size of canvas
	
		frame = new JFrame(NAME); //actually creates JFrame
		
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);//sets the default close operation
		frame.setLayout(new BorderLayout());//creates border layout of frame
		
		frame.add(this, BorderLayout.CENTER);//sets game to frame
		frame.pack();//sizes everything correctly at or above preferred size
		
		frame.setResizable(false);//makes it so frame is not resizable
		frame.setLocationRelativeTo(null);//set to null because we don't want anything set to relative location
		frame.setVisible(true);//sets the frame to be visible
		
		
	}
	
	public synchronized void start() { //call this function in the applett
		running = true;//set running to true (game is running)
		new Thread(this).start(); //thread is an instance of runnable, prevents from taking away from system main thread
		
	}
	
	public synchronized void stop() { 
		running = false;//set running to false (game is stopped)
		
		
	}
	
	public void run() { //should be overidden but idk
		long lastTime = System.nanoTime(); //gets time in nano seconds from the epoch
		double nanoPerTick = 1000000000/60D;//how many nanoseconds per tick
		
		int frames = 0;
		int ticks = 0;
		long lastTimer = System.currentTimeMillis(); //this is to makesure we are only getting one frame per second
		
		double delta = 0;//how many unprocessed nanoseconds have gone by
		
		while(running) { //while the game is running
			long now = System.nanoTime();
			delta += (now - lastTime) / nanoPerTick;
			lastTimer = now;//prevents lastTimer from being static
			boolean isRender = true;//run loop to allow render
			
			
			while(delta >= 1) {
			ticks++;//adds one to tick
			tick();//everytime we are running update tick and render
			delta -= 1;//continues until it hits the nanoPerTick threshold
			isRender = true;
			}
			
			try {
				Thread.sleep(2);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
			if(isRender) {
			frames++;
			render();
			}
			
			if (System.currentTimeMillis() - lastTimer >= 1000) { //resets the variable so it doesn't increase unto infinity (1000 milliseconds in one second)
				lastTimer += 1000;
				frames = 0;
				ticks = 0;
				
			}
		}
		
	}
	
	public void tick() {
		tickCount++;
		
	}
	
	public void render() {
		BufferStrategy bs = getBufferStrategy();//object to organize data displayed
		if (bs == null) {
			createBufferStrategy(3);//triple buffering (tearing image prevention)
			return;
		}
		
		Graphics g = bs.getDrawGraphics(); //creates graphics object
		
		g.setColor(Color.BLACK);
		g.fillRect(0, 0, getWidth(), getHeight());
		
		g.drawImage(image, 0, 0, getHeight(), getWidth(), null);//draws the image object
		
		g.dispose();//dispose the graphics and free up memory
		bs.show();//show buffer
	}
	
	public static void main(String[] args) { //main here
		new Game().start();
		
	}


}
