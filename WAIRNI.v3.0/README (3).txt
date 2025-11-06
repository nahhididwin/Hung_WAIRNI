Project : Hung_WAIRNI method
(GitHub : https://github.com/nahhididwin/Hung_WAIRNI)

Creator : Đặng Đình Phú Hưng (Dang Dinh Phu Hung, https://github.com/nahhididwin)
Date of birth (Dang Dinh Phu Hung) (DD/MM/YYYY): 20/06/2011
Languages ​​used: English.
Publication date (DD/MM/YYYY): 

This project uses this license (MIT LICENSE) : https://github.com/nahhididwin/Hung_WAIRNI/blob/main/LICENSE

--
Main parts:
0. What is this project for?
1. The "split" problem, asymmetry, and related data types
2. Important things
3. Find "peak" with Hung_WAIRNI
4. Calculate area (Numerical integration) with Hung_WAIRNI
5. Error analysis
6. Comparison with other methods/algorithms
7. Deploy with code
8. End
--


--
0. What is this project for?
A method called "Hung_WAIRNI", is a numerical integration method, a peak finding method, used to find "peaks", calculate "areas". Optimized to suit the specific task, currently according to the author, is suitable for AC power.
--

--
1. The "split" problem, asymmetry, and related data types
--

--
2. Important things


The IEEE standard (519) is an international standard, very popular, almost mandatory, and it is the standard for "harmonics" for AC electrical signals.

IEEE 519 : https://www.elspec-ltd.com/ieee-519-2014-standard-for-harmonics/

Okay, let's keep this project focused on an AC signal with Total Harmonic Distortion (THD) <= 8% (That is from 0% -> 8%. I have not looked into higher levels). And this THD level is EXTREMELY COMMON.

Okay, let's look at the image p98.png (https://github.com/nahhididwin/Hung_WAIRNI/blob/main/WAIRNI.v3.0/p98.png), source here: https://www.elspec-ltd.com/wp-content/uploads/2021/01/illustration2-1-800x387.png

The AC signal would theoretically be a pure sine wave. But with real data, the sine wave is "distorted" due to many problems.

You can see, although it is affected by "harmonics", but because THD <= 8%, it mostly affects the "peak" part of half the sine wave cycle, and slightly affects the part near "0 V".

Why?
Harmonic distortion is the presence of frequencies that are integer multiples of the fundamental frequency (usually 50 Hz or 60 Hz) in a signal.

Fundamental Frequency: The original frequency, which forms the main sine wave shape (e.g., 50 Hz).

Harmonics: Multiple frequencies (e.g. 3rd harmonic is 3 * 50 Hz = 150 Hz, 5th harmonic is 5 * 50 Hz = 250 Hz, etc.).

Low order harmonics (mainly 3rd, 5th, 7th) are the main cause of distortion:
In most power systems, the lower odd order harmonics such as 3rd, 5th, and 7th are the components with the largest amplitude and cause the most significant distortion.
Higher order harmonics (such as 11th, 13th, etc.) are usually very small in amplitude and have less effect.

Superposition of 3rd and 5th Harmonics:
3rd Harmonics: Have the greatest impact on the peak of the sine wave. When 3rd harmonics (which pass through zero at points other than the fundamental) are added, they tend to flatten or sharpen the peak of the 50 Hz sine wave.
5th Harmonics: The 5th harmonic often causes “notching” on the slope of the wave, but it still contributes to the overall distortion, especially around the peak.

Low harmonic amplitude:
Since the THD is only 8% (an acceptable limit in many industry standards), this means that the total energy of all the higher harmonics is only 8% of the energy of the fundamental.
Due to their small amplitudes, the higher harmonics are not strong enough to completely change the shape of the sine wave, but only enough to cause local distortion in the regions where the fundamental is at its maximum value (the peak region). At the peak region, the fundamental is changing the slowest (derivative is close to 0), so the superposition of small-amplitude harmonics is more likely to distort the shape there.



--
