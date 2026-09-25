# 🌙 Moon Position Calculator

A lightweight, web-based astronomical calculator that determines the position, phase, and rise/set times of the moon for a specific date, time, and location. Built completely with HTML, CSS, and vanilla JavaScript—no external dependencies required.

Try here:
🌙 [Moon Position Calculator](https://yonigr94.github.io/moon_loc_calc/)

## ✨ Features

* **High-Accuracy Astronomical Data**: Calculates the moon's altitude and azimuth using a robust orbital model that includes major perturbations (Evection, Variation).
* **Atmospheric & Optical Corrections**: Automatically adjusts for lunar parallax and atmospheric refraction to provide precise horizon-crossing times.
* **Geometric Moon Phases**: Generates an SVG graphic representing the moon phase calculated directly from real-time Sun-Moon geometric elongation.
* **Rise and Set Times**: Calculates local moonrise and moonset events. *(Note: Because a lunar day is about 24 hours and 50 minutes long, there is usually one calendar day each month where the moon skips a rise or set event!)*
* **Multi-Language & Hemisphere Support**: Full localization in English, Hebrew, and Arabic, featuring automatic RTL/LTR layout switching and dynamic phase orientation for the Southern Hemisphere.

## 🚀 Getting Started

Zero build process—simply download and open `index.html` in any modern web browser to run the application locally.

## 📖 The Story Behind It

Ever since I was young, it always surprised me that—despite children's books and TV shows teaching us that the sun is for the day and the moon is for the night—I would often spot the moon in broad daylight, shining stark white against a clear blue sky.

Now that I’m a parent, my own children notice the daytime moon too, whether it’s 8:00 AM or early evening before the sun has even set. That’s just how kids are: they look at the world exactly as it is and speak whatever is on their minds.

I decided to create a tool that calculates exactly where the moon will be in the sky at any given moment, along with its daily rise and set times. I built it for one simple reason: to point out the moon to my kids. But you might find an entirely different use for it—this calculator is open for anyone to use and enjoy.

## 🔭 Inspiration for Use

* **Astrophotography & Stargazing**: Predict when and where the moon will rise above a specific horizon, or figure out if the moon's brightness will wash out a meteor shower.
* **Educational Tool**: Use the open-source code as a sandbox to learn how mathematical coordinate transformations, Keplerian mechanics, and atmospheric optics translate into code.
* **Outdoor & Navigation**: Understand nighttime illumination for camping, night hiking, or sailing.

## 🧮 The Math Behind the Magic

The engine of this calculator relies on converting standard time into astronomical metrics, applying orbital perturbations, and adjusting for optics. Here is a simplified breakdown:

### 1. The Independent Variable: Time ($d$)
The primary variable is time, converted to $d$ – the number of days passed since the astronomical reference date known as J2000.0 (January 1, 2000, at exactly 12:00 TT/Noon):
$$d = \frac{\text{time in ms}}{86400000} - 10957.5$$

### 2. Linear Equations & Perturbations (The Orbit)
We first calculate the mean position of the Moon and the Sun. Because the Moon's orbit is heavily influenced by the Sun's gravity and is highly elliptical, we apply several major trigonometric corrections to its longitude ($l$):
* **Equation of the Center ($6.289^\circ$):** Corrects for the elliptical shape of the orbit.
* **Evection ($1.274^\circ$):** Corrects for the Sun's gravitational pull altering the eccentricity of the Moon's orbit.
* **Variation ($0.658^\circ$):** Corrects for the Moon speeding up as it approaches New/Full phases and slowing down at the quarters.
* **Annual Equation ($0.186^\circ$):** Adjusts for Earth's varying distance from the Sun throughout the year.

### 3. Coordinate Transformation (Space to Equatorial)
The model transforms these Ecliptic coordinates into the Equatorial system (Right Ascension and Declination) using Earth's axial tilt ($e \approx 23.44^\circ$). Finally, using the user's specific latitude and longitude, spherical trigonometry determines the exact local **Altitude** and **Azimuth**.

### 4. Optical Corrections (Parallax & Refraction)
The position in space doesn't perfectly match what human eyes see from the surface. We apply two critical corrections to the altitude:
* **Lunar Parallax:** Because the moon is relatively close to Earth, viewing it from the planet's surface (rather than its center) "pulls" the moon down by about $0.95^\circ$ at the horizon.
* **Atmospheric Refraction:** Earth's atmosphere acts like a lens, bending light and "lifting" the apparent position of the moon by roughly $0.5^\circ$ as it crosses the horizon. 
Together, these ensure highly accurate rise and set times.

### 5. True Geometric Phase
Instead of estimating the phase using an average 29.53-day cycle, the application calculates the phase dynamically by finding the exact longitudinal angle (elongation) between the Moon and the Sun in real-time.

## ⚠️ Limitations

While this tool offers excellent accuracy for general astronomy and planning, keep the following in mind:

1. **Truncated Model:** The algorithm includes the largest perturbation terms (Evection, Variation) but omits dozens of minor terms found in the full ELP2000 or full Meeus models. Altitude/Azimuth accuracy is generally within ~0.2°, which is sufficient for most uses but not for strict scientific ephemerides.
2. **Hardcoded Locations:** Currently, the tool relies on a fixed dictionary of major cities rather than supporting dynamic GPS coordinates or arbitrary search functionality.
3. **Phase Visuals:** The moon phase graphic uses a simplified SVG scaling transformation (and flips horizontally for the Southern Hemisphere) rather than a physically precise terminator line projection.

## 📄 License & Credits

* **Project License**: [MIT License](LICENSE) (Open for anyone to use and modify).
* **Astronomical Calculations**: Core celestial elements are derived from [SunCalc v1.x](https://github.com/mourner/suncalc) (Copyright (c) 2014, Vladimir Agafonkin, BSD-2-Clause License), which is based on Jean Meeus' *Astronomical Algorithms*. This project extends those core elements with larger perturbation terms, optical corrections, and sun-based geometric phases for enhanced precision.

## 👨‍💻 Author

**Yoni Getahun**
* [GitHub](https://github.com/YoniGR94)
* [LinkedIn](https://www.linkedin.com/in/yoni-getahun/)
