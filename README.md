# 🌙 Moon Position Calculator

A lightweight, web-based astronomical calculator that determines the approximate position, phase, and rise/set times of the moon for a specific date, time, and location. Built completely with HTML, CSS, and vanilla JavaScript—no external dependencies required.

Try here:
🌙 [Moon Position Calculator](https://yonigr94.github.io/moon_loc_calc/)

## ✨ Features

* **Astronomical Data**: Calculates the moon's altitude (height above the horizon) and azimuth (compass direction) based on a simplified orbital model.
* **Dynamic Moon Phases**: Generates an SVG graphic representing the current moon phase based on the average synodic month.
* **Rise and Set Times**: Calculates local moonrise and moonset events for the selected day. *(Note: Because a lunar day is about 24 hours and 50 minutes long, the moon rises roughly 50 minutes later each day. As a result, there is usually one calendar day each month where the moon simply skips a rise or set event!)*
* **Built-in Time Zone Handling**: Handles complex time zone conversions directly in the browser for predefined locations.
* **Multi-Language Support**: Full localization in English, Hebrew, and Arabic, featuring automatic RTL/LTR layout switching.

## 🚀 Getting Started

Zero build process—simply download and open `index.html` in any modern web browser to run the application locally.

## 📖 The Story Behind It

Ever since I was young, it always surprised me that—despite children's books and TV shows teaching us that the sun is for the day and the moon is for the night—I would often spot the moon in broad daylight, shining stark white against a clear blue sky.

Now that I’m a parent, my own children notice the daytime moon too, whether it’s 8:00 AM or early evening before the sun has even set. That’s just how kids are: they look at the world exactly as it is and speak whatever is on their minds.

Something else that changed as I grew older is that my ability to build software has improved exponentially, thanks to LLMs. So, I decided to create a tool that calculates roughly where the moon will be in the sky at any given moment, along with its daily rise and set times.

I built it for one simple reason: to point out the moon to my kids. But you might find an entirely different use for it—this calculator is open for anyone to use and enjoy.

## 🔭 Inspiration for Use

* **Casual Stargazing & Astronomy**: Figure out if the moon's brightness will wash out a meteor shower on a given night, or find a good time to observe craters through a telescope.
* **Educational Tool**: Use the open-source code as a sandbox to learn how mathematical coordinate transformations and Keplerian mechanics translate into code.
* **Outdoor & Navigation**: Understand nighttime illumination for camping, night hiking, or sailing.

## 🧮 The Math Behind the Magic

The engine of this calculator relies on converting standard time into astronomical metrics and applying orbital equations. Here is a simplified breakdown of the core math used in the code:

### 1. The Independent Variable: Time ($d$)
The primary and only variable in our moon model is time. In the code, the function `toDays(dateValue)` takes a timestamp and converts it to $d$ – the number of days passed since the astronomical reference date known as J2000.0 (January 1, 2000, at exactly 12:00 TT/Noon). This 12-hour offset is why the formula subtracts 10957.5 rather than exactly 10957:
$$d = \frac{\text{time in ms}}{86400000} - 10957.5$$

### 2. Linear Equations (Mean Elements)
In the first step of the calculation, we assume the moon moves in a uniform circular motion. We calculate three base angles:
* **$L$ (Mean Longitude):** $$L = 218.316^\circ + 13.176396^\circ \cdot d$$
* **$M$ (Mean Anomaly):** $$M = 134.963^\circ + 13.064993^\circ \cdot d$$
* **$F$ (Argument of Latitude):** $$F = 93.272^\circ + 13.229350^\circ \cdot d$$

The coefficient $13.17^\circ$ represents the moon's eastward movement relative to the background stars each day.

### 3. Trigonometric Corrections (Perturbations)
In reality, the moon's orbit is an ellipse, so its orbital speed varies (Kepler's laws). We use a trigonometric series to add "perturbations":

* **$l$ (Actual Longitude):**
  $$l = L + 6.289^\circ \sin(M)$$
  The $6.289^\circ$ coefficient comes directly from the eccentricity of the moon's orbit ($e \approx 0.0549$). In radians, this main correction term is $2e$, which evaluates to roughly $6.29^\circ$ ($2 \times 0.0549 \times 57.3^\circ$).
  
* **$b$ (Actual Latitude):**
  $$b = 5.128^\circ \sin(F)$$
  The moon's orbit is tilted relative to the ecliptic plane by about $5.1^\circ$.

* **$dt$ (Distance - Earth-to-Moon distance in km):**
  $$dt = 385001 - 20905 \cos(M)$$
  Because the orbit is elliptical, this distance varies as a cosine function (this fluctuation creates the "Supermoon" phenomenon).

### 4. Coordinate Transformation (Space to Equatorial System)
The math gives us the moon's position relative to the Ecliptic. We perform a coordinate transformation to the Equatorial system using Earth's axial tilt constant ($e \approx 23.44^\circ$):
$$\alpha = \arctan2(\sin(l) \cos(e) - \tan(b) \sin(e), \cos(l))$$
$$\delta = \arcsin(\sin(b) \cos(e) + \cos(b) \sin(e) \sin(l))$$

From there, the application uses **spherical trigonometry** (specifically calculating the local hour angle: $H = \text{sidereal time} - \alpha$) to combine these spherical coordinates with the specific latitude and longitude of the user's city, finding the exact local viewing angles: **Altitude** and **Azimuth**.

## ⚠️ Limitations

While this tool is great for casual observation and education, it is not designed for high-precision astrophotography planning:

1. **Astronomical Accuracy**: The calculation uses a simplified model based on SunCalc 1.x. It omits dozens of lunar perturbation terms (such as evection and variation). As a result, the altitude error averages ~0.73° (up to 2.45°—which is about 3-4 moon diameters!), and azimuth errors can spike significantly (up to 19°) when the moon is near the zenith.
2. **Refraction & Parallax**: The model does not fully correct for atmospheric refraction or lunar parallax. Because the moon is relatively close to Earth, lunar parallax shifts its apparent position by roughly 0.95° at the horizon. This missing correction contributes to an error of about 3 to 8 minutes in rise and set times.
3. **Average Moon Phase**: The phase is not calculated from exact real-time sun-moon geometry. Instead, it uses an average synodic month (29.53 days) starting from a J2000.0 epoch (Jan 6, 2000). The true astronomical phase can deviate from this average by up to ~14 hours.
4. **Hardcoded Locations**: Currently, the tool relies on a fixed dictionary of major cities rather than supporting dynamic GPS coordinates or arbitrary search functionality.
5. **Phase Visuals**: The moon phase graphic uses a simplified SVG scaling transformation to adapt between the Northern and Southern hemispheres, rather than a physically precise terminator line projection.

## 📄 License & Credits

* **Project License**: [MIT License](LICENSE) (Open for anyone to use and modify).
* **Astronomical Calculations**: The lunar math engine is based on [SunCalc v1.x](https://github.com/mourner/suncalc) (Copyright (c) 2014, Vladimir Agafonkin, released under the BSD-2-Clause License), which derives its simplified formulas from Jean Meeus' highly regarded book, *Astronomical Algorithms*. *(Note: SunCalc v2.0 offers higher precision, but this tool implements the lightweight v1.x model).*

## 👨‍💻 Author

**Yoni Getahun**
* [GitHub](https://github.com/YoniGR94)
* [LinkedIn](https://www.linkedin.com/in/yoni-getahun/)
