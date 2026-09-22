# 🌙 Moon Position Calculator

A lightweight, web-based astronomical calculator that determines the exact position, phase, and rise/set times of the moon for a specific date, time, and location. Built completely with HTML, CSS, and vanilla JavaScript—no external dependencies required.

Try here:
🌙 [Moon Position Calculator](https://yonigr94.github.io/moon_loc_calc/)

## ✨ Features

* **Real-Time Astronomical Data**: Accurately calculates the moon's altitude (height above the horizon) and azimuth (compass direction).
* **Dynamic Moon Phases**: Generates an SVG graphic representing the current moon phase (e.g., Waxing Crescent, Full Moon) based on the exact timestamp.
* **Appearing Tracking**: Calculates local moonrise and moonset times.
* **Built-in Time Zone Handling**: Handles complex time zone conversions directly in the browser for predefined locations (e.g., Tel Aviv, New York, Vienna, Nicosia).
* **Multi-Language Support**: Full localization in English, Hebrew, and Arabic, featuring automatic RTL/LTR layout switching.

---

## 📖 The Story Behind It

Ever since I was young, it always surprised me that—despite children's books and TV shows teaching us that the sun is for the day and the moon is for the night—I would often spot the moon in broad daylight, shining stark white against a clear blue sky.

Now that I’m a parent, my own children notice the daytime moon too, whether it’s 8:00 AM or early evening before the sun has even set. That’s just how kids are: they look at the world exactly as it is and speak whatever is on their minds.

Something else that changed as I grew older is that my ability to build software has improved exponentially, thanks to LLMs. So, I decided to create a tool that calculates exactly where the moon will be in the sky at any given moment, along with its daily rise and set times.

I built it for one simple reason: to point out the moon to my kids. But you might find an entirely different use for it—this calculator is open for anyone to use and enjoy.

---

## 🚀 Inspiration for Use

* **Astrophotography Planning**: Predict exactly when and where the moon will rise above a specific landmark or horizon to get the perfect shot.
* **Stargazing & Astronomy**: Figure out if the moon's brightness will wash out a meteor shower on a given night, or find the best time to observe craters through a telescope.
* **Educational Tool**: Use the open-source code as a sandbox to learn how mathematical coordinate transformations and Keplerian mechanics translate into code.
* **Outdoor & Navigation**: Understand nighttime illumination for camping, night hiking, or sailing.

---

## 🧮 The Math Behind the Magic

The engine of this calculator relies on converting standard time into astronomical metrics and applying orbital equations. Here is a simplified breakdown of the core math used in the code:

### 1. The Independent Variable: Time ($d$)
The primary and only variable in our moon model is time. In the code, the function `toDays(dateValue)` takes a timestamp and converts it to $d$ – the number of days passed since the astronomical reference date known as J2000.0 (January 1, 2000):
$$d = \frac{\text{time in ms}}{86400000} - 10957.5$$

### 2. Linear Equations (Mean Elements)
In the first step of the calculation, we assume the moon moves in a uniform circular motion. We calculate three base angles (in the source code, coefficients are converted to radians, but here we present them in degrees for convenience):

* **$L$ (Mean Longitude):** The angular position of the moon along its orbit if it were moving at a perfectly constant speed.
  $$L = 218.316^\circ + 13.176396^\circ \cdot d$$
* **$M$ (Mean Anomaly):** The angle describing the moon's position relative to the point in its orbit closest to Earth (perigee).
  $$M = 134.963^\circ + 13.064993^\circ \cdot d$$
* **$F$ (Argument of Latitude):** The angular distance of the moon from the point where it crosses the ecliptic plane (the plane where Earth orbits the sun).
  $$F = 93.272^\circ + 13.229350^\circ \cdot d$$

*Meaning of the parameters (equations in the standard form $y = mx + b$):*
* **The constants ($b$):** Numbers like $218.316^\circ$ are initial conditions – the exact position of the moon at the J2000.0 reference moment.
* **The coefficients of $d$ ($m$):** The first derivative of position with respect to time, representing the daily angular velocity. The moon travels across the sky by about $13.17^\circ$ each day.

### 3. Trigonometric Corrections (Perturbations)
In reality, the moon's orbit is not a perfect circle but an ellipse, so its orbital speed varies (Kepler's laws). We use the largest terms of a trigonometric series to add "perturbations" that correct the orbit from its linear path:

* **$l$ (Actual Longitude):**
  $$l = L + 6.289^\circ \sin(M)$$
  We add a sine wave. The $6.289^\circ$ coefficient comes from the eccentricity (flattening) of the moon's orbit. As $M$ increases, the expression oscillates and "corrects" the position forward and backward relative to the mean position $L$.
  
* **$b$ (Actual Latitude):**
  $$b = 5.128^\circ \sin(F)$$
  The moon's orbit is tilted relative to the ecliptic plane. This equation describes the moon's rise and fall within a range of about $5.1^\circ$ above and below the plane.

* **$dt$ (Distance - Earth-to-Moon distance in km):**
  $$dt = 385001 - 20905 \cos(M)$$
  The moon's mean distance from Earth is the constant 385,001 km. Because the orbit is elliptical, this distance varies as a cosine function with an amplitude of 20,905 km (this fluctuation is what creates the "Supermoon" phenomenon).

### 4. Coordinate Transformation (Space to Equatorial System)
So far, the math has given us the moon's position relative to the solar system's plane (Ecliptic). The final functions in the code (`rightAscension` and `declination`) take the angles $l$ and $b$, and together with Earth's axial tilt constant ($e \approx 23.44^\circ$), perform a spherical coordinate transformation to the Equatorial system that Earth actually "sees":

$$\alpha = \arctan2(\sin(l) \cos(e) - \tan(b) \sin(e), \cos(l))$$
$$\delta = \arcsin(\sin(b) \cos(e) + \cos(b) \sin(e) \sin(l))$$

From there, the application takes these spherical coordinates ($\alpha, \delta$) and combines them with the specific latitude and longitude of the user's city using vector algebra to find the exact local viewing angles: **Altitude** and **Azimuth**.

---

## 🔭 Credits & Inspiration

**Astronomical calculations:** The lunar position calculations are based on the algorithms used by [SunCalc](https://github.com/mourner/suncalc), which in turn are based on formulas from Jean Meeus' highly regarded book, *Astronomical Algorithms*.
