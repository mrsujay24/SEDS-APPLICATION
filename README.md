# SEDS-APPLICATION
This is my submission for SEDS Avionics entry. My name is Sujay. My ID is 2026A4PS0599H. My BITS email is f20260599@hyderabad.bits-pilani.ac.in
Link to my TINKERCAD work: https://www.tinkercad.com/things/ilfhYAcAhpV/editel?returnTo=%2Fdashboard%2Fclasses&sharecode=HNS3yJgtAe5fgrEfuRFVViNKROp29L_ubNmarSOE0zw


Here is a detailed breakdown of how the script for TASK 1 operates, section by section:

1. Data Ingestion and Preparation
•	Library Imports: The script loads Pandas for structured data handling, Matplotlib for visualization, and FuncAnimation to generate dynamic motion.
•	Loading the CSV: It reads a file named "Depth Data.csv" and extracts two key columns: Point (representing the time sequence) and Depth (m) (the raw depth measurements).

2. Data Cleaning and Signal Smoothing
Real-world sensor data is often noisy or contains missing values. The script handles this with three sequential steps:


•	Coercing Errors: pd.to_numeric(..., errors='coerce') converts any accidental text strings, corrupted readings, or blank spaces in the raw data into NaN (Not a Number) values so they don't break subsequent calculations.
•	Interpolation & Filling: .interpolate(method='linear').bfill().ffill() estimates missing gaps using a straight line between adjacent points. Backward and forward fills ensure that any missing values at the very beginning or end of the dataset are covered
•	Rolling Average Filter: .rolling(window=5, center=True, min_periods=1).mean() applies a moving average window of 5 data points. This smooths out abrupt noise, spikes, or erratic sensor blips to reveal a clean depth trend.

3. Matplotlib Figure & Dynamic Layout Setup
•	Canvas Initialization: A figure and axis object (fig, ax) are created with a defined size (10x6 inches).

•	Dynamic Scaling: Instead of guessing the bounds, the script automatically calculates the minimum and maximum values of the time and smoothed depth. It adds a safety padding (either 10% of the depth range or a minimum of 10 meters) so the plotted line never awkwardly clips against the edges of the graph.
•	Aesthetics: It applies grid lines (linestyle='--'), a custom title using the GRAPH_TITLE variable, axis labels, and an interactive legend.

4. Animation Frame Execution
•	Empty Line Initialization: It creates an empty plot line (ax.plot([], [])) with a royal blue color and a thickness of 2 pixels
•	The Update Function: def update(frame): acts as the engine for every frame of the animation. At each step, it uses iloc[:frame+1] to progressively slice the time and depth arrays from index 0 up to the current frame, updating the line data dynamically.
•	FuncAnimation Loop: The animation calls the update function across every frame in the dataset (len(time)) with a 50-millisecond delay between frames. blit=False is used to ensure reliable rendering across different Python environments (like IDLE), and repeat=False stops the animation once it reaches the end
•	Rendering: plt.show(block=True) opens the interactive desktop window, locking execution until the window is closed.


This is how my program for TASK 2 functions.
•	State Machine: The program uses five states: OPEN SEA, ANCHOR DROPPED, STORM, CHARYBDIS, and WRECKED. The ship begins in OPEN SEA.
•	Light Sensor: The light sensor is connected to A0. When the light level falls below the halfway threshold, the ship enters STORM. When the brightness rises above the threshold before five seconds, it returns to OPEN SEA and the timer resets.
•	Storm Warning: While the ship is in STORM, the LED connected to D9 blinks every 500 milliseconds to indicate dangerous weather.
•	Ultrasonic Sensor: The PING))) sensor on D7 continuously measures distance. If it detects an object closer than 100 cm, the ship enters CHARYBDIS.
•	Charybdis Warning: While in CHARYBDIS, the buzzer connected to D8 sounds. If the distance becomes 100 cm or greater before five seconds, the ship returns to OPEN SEA.
•	Danger Timer: millis() is used to measure how long the ship remains in STORM or CHARYBDIS. If either danger continues for 5 seconds, the ship becomes WRECKED.
•	Anchor System: The push button on D10 toggles the anchor. When the anchor is dropped, the state becomes ANCHOR DROPPED, all warnings stop, and the danger timer is reset. The ship cannot be wrecked while the anchor is down.
•	Wrecked State: Once the ship enters WRECKED, it cannot return to another state. The simulation must be restarted to reset the ship.
•	LCD Display: The I2C LCD displays the ship's current state and is updated whenever the state changes.
•	Overall Program Flow: The Arduino repeatedly reads the sensors → checks the anchor button → updates the state → controls the LED/buzzer → updates the LCD.
