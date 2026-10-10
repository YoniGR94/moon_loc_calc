# 🌙 Moon Position Calculator

A lightweight, web-based astronomical calculator that determines the position, phase, and rise/set times of the moon for a specific date, time, and location[cite: 3]. Built completely with HTML, CSS, and vanilla JavaScript—no external dependencies or build processes required[cite: 3].

Try here:
🌙 [Moon Position Calculator](https://yonigr94.github.io/moon_loc_calc/)

## ✨ Features

* **Interactive Map & Custom Locations**: Choose from a predefined list of major cities, or toggle the interactive world map (powered by Leaflet) to pick any exact coordinate. The map and its assets are lazy-loaded only when requested to save bandwidth.
* **Automatic Timezones**: Dynamically calculates the correct timezone for any custom location selected on the map using local offline lookup (`tz-lookup`).
* **Elevation Support**: Input your elevation above sea level to adjust the visible horizon, calculating highly precise moonrise and moonset times for mountainous or elevated observer locations.
* **High-Accuracy Astronomical Data**: Calculates the moon's altitude and azimuth using a robust orbital model that includes major perturbations (Evection, Variation)[cite: 3].
* **Atmospheric & Optical Corrections**: Automatically adjusts for lunar parallax and atmospheric refraction to provide precise horizon-crossing times[cite: 3].
* **Geometric Moon Phases**: Generates an SVG graphic representing the moon phase calculated directly from real-time Sun-Moon geometric elongation[cite: 3].
* **Multi-Language & Hemisphere Support**: Full localization in English, Hebrew, and Arabic, featuring automatic RTL/LTR layout switching and dynamic phase orientation for the Southern Hemisphere[cite: 3].

## 🚀 Getting Started

Zero build process—simply download and open `index.html` in any modern web browser to run the application locally[cite: 3].

## 📖 The Story Behind It

Ever since I was young, it always surprised me that—despite children's books and TV shows teaching us that the sun is for the day and the moon is for the night—I would often spot the moon in broad daylight, shining stark white against a clear blue sky[cite: 3].

Now that I’m a parent, my own children notice the daytime moon too, whether it’s 8:00 AM or early evening before the sun has even set[cite: 3]. That’s just how kids are: they look at the world exactly as it is and speak whatever is on their minds[cite: 3].

I decided to create a tool that calculates exactly where the moon will be in the sky at any given moment, along with its daily rise and set times[cite: 3]. I built it for one simple reason: to point out the moon to my kids. But you might find an entirely different use for it—this calculator is open for anyone to use and enjoy[cite: 3].

## 🔭 Inspiration for Use

* **Astrophotography & Stargazing**: Predict when and where the moon will rise above a specific horizon, or figure out if the moon's brightness will wash out a meteor shower[cite: 3].
* **Educational Tool**: Use the open-source code as a sandbox to learn how mathematical coordinate transformations, Keplerian mechanics, and atmospheric optics translate into code[cite: 3].
* **Outdoor & Navigation**: Understand nighttime illumination for camping, night hiking, or sailing[cite: 3].

## 🧮 The Math Behind the Magic

The engine of this calculator relies on converting standard time into astronomical metrics, applying orbital perturbations, and adjusting for optics[cite: 3]. Here is a simplified breakdown:

### 1. The Independent Variable: Time ($d$)
The primary variable is time, converted to $d$ – the number of days passed since the astronomical reference date known as J2000.0 (January 1, 2000, at exactly 12:00 TT/Noon)[cite: 3]:
$$d = \frac{\text{time in ms}}{86400000} - 10957.5$$

### 2. Linear Equations & Perturbations (The Orbit)
We first calculate the mean position of the Moon and the Sun[cite: 3]. Because the Moon's orbit is heavily influenced by the Sun's gravity and is highly elliptical, we apply several major trigonometric corrections to its longitude ($l$)[cite: 3]:
* **Equation of the Center ($6.289^\circ$):** Corrects for the elliptical shape of the orbit[cite: 3].
* **Evection ($1.274^\circ$):** Corrects for the Sun's gravitational pull altering the eccentricity of the Moon's orbit[cite: 3].
* **Variation ($0.658^\circ$):** Corrects for the Moon speeding up as it approaches New/Full phases and slowing down at the quarters[cite: 3].
* **Annual Equation ($0.186^\circ$):** Adjusts for Earth's varying distance from the Sun throughout the year[cite: 3].

### 3. Coordinate Transformation (Space to Equatorial)
The model transforms these Ecliptic coordinates into the Equatorial system (Right Ascension and Declination) using Earth's axial tilt ($e \approx 23.44^\circ$)[cite: 3]. Finally, using the user's specific latitude and longitude, spherical trigonometry determines the exact local **Altitude** and **Azimuth**[cite: 3].

### 4. Optical Corrections (Parallax & Refraction)
The position in space doesn't perfectly match what human eyes see from the surface[cite: 3]. We apply two critical corrections to the altitude:
* **Lunar Parallax:** Because the moon is relatively close to Earth, viewing it from the planet's surface (rather than its center) "pulls" the moon down by about $0.95^\circ$ at the horizon[cite: 3].
* **Atmospheric Refraction:** Earth's atmosphere acts like a lens, bending light and "lifting" the apparent position of the moon by roughly $0.5^\circ$ as it crosses the horizon[cite: 3]. 
Together, these ensure highly accurate rise and set times[cite: 3].

### 5. True Geometric Phase
Instead of estimating the phase using an average 29.53-day cycle, the application calculates the phase dynamically by finding the exact longitudinal angle (elongation) between the Moon and the Sun in real-time[cite: 3].

## ⚠️ Limitations

While this tool offers excellent accuracy for general astronomy and planning, keep the following in mind[cite: 3]:

1. **Truncated Model:** The algorithm includes the largest perturbation terms (Evection, Variation) but omits dozens of minor terms found in the full ELP2000 or full Meeus models[cite: 3]. Altitude/Azimuth accuracy is generally within ~0.2°, which is sufficient for most uses but not for strict scientific ephemerides[cite: 3].
2. **Phase Visuals:** The moon phase graphic uses a simplified SVG scaling transformation (and flips horizontally for the Southern Hemisphere) rather than a physically precise terminator line projection[cite: 3].

## 📄 License & Credits

* **Project License**: [MIT License](LICENSE) (Open for anyone to use and modify)[cite: 3].
* **Astronomical Calculations**: Core celestial elements are derived from [SunCalc v1.x](https://github.com/mourner/suncalc) (Copyright (c) 2014, Vladimir Agafonkin, BSD-2-Clause License), which is based on Jean Meeus' *Astronomical Algorithms*[cite: 3]. This project extends those core elements with larger perturbation terms, optical corrections, and sun-based geometric phases for enhanced precision[cite: 3].
* **Map & Timezones**: Interactive mapping provided by Leaflet and dark tiles by Esri. Offline coordinate-to-timezone resolution provided by `tz-lookup`.

## 👨‍💻 Author

**Yoni Getahun**
* [GitHub](https://github.com/YoniGR94)[cite: 3]
* [LinkedIn](https://www.linkedin.com/in/yoni-getahun/)[cite: 3]
