General Mission Analysis Tool for Amateur Radio Exploration
June 17, 2020

Thanks to Achim Vollhardt's sharp eyes and quick work, we obtained a script and a SPICE kernel file (*.bsp) for the Lunar Gateway running in NASA's open source General Mission Analysis Tool (GMAT). 
Thank you to Michael KK6OOZ for configuring and recording animations and interpreting the results. 

Installation and documentation:
http://gmatcentral.org/display/GW/GMAT+Wiki+Home
https://sourceforge.net/projects/gmat/files/GMAT
https://documentation.help/GMAT/Preface.html

The SPICE kernel file (these type of files end in .bsp) can be found here:
https://naif.jpl.nasa.gov/pub/naif/misc/MORE_PROJECTS/DSG/

The original GMAT script from Achim is in Appendix A. Math for Az El and Distance was added and that script is in Appendix B. The Gateway-centric view GMAT script is in Appendix C. A script with additional orbital views from Ken Ernandes is in Appendix D. 

This write-up walks through how to use the scripts and summarizes some of the information that can be learned from “propagating the orbit” in GMAT. 

----

What is GMAT? 

GMAT is an open source software tool from NASA that provides mission analysis. It has been publicly and actively developed. With GMAT, one can simulate published orbits, specify custom orbits, design transfer trajectories, and plan maneuvers. It was intended to provide mission analysis, orbital regimes, a graphical user interface, scripts as inputs, high precision, and is fully tested. 
GMAT is high fidelity and is operationally certified (ACE) for NASA missions. GMAT is desktop oriented, meaning it is not a batch job or a console application. 

What is a SPICE kernel file?  

One of the required elements to simulate our orbit in GMAT is a binary SPK (bsp) file. SPK stands for SPICE kernel file.
SPICE is an observation geometry system for space science missions. It allows you to find when specified “geometric events” occur. For example, occultations, when a particular range is reached or exceeded, or when a spacecraft is pointing at an object.
The SPICE acronym comes from Spacecraft, Planet, Instrument, “C-matrix”, and Events. 
SPICE is used for both planning missions and for archiving mission data. 
To make kernels available to a program you “load” them. There are text and binary kernel types. The bsp file we have is a binary kernel. A binary kernel contains lots of non-printing data, but it also includes a “comment area” where descriptive meta-data provided as ASCII text is should be placed. Text kernels have comments interleaved with the data. 
Fortunately, the binary kernel file we have uses the comment area. Here is the comment, cleaned up a bit for readability.

Sample Deep Space Gateway orbit file, produced to help scientists and rthernble enough SPICE ID for the DSG platform (the spacecraft) is -60000. 

Binary kernels are made using Toolkit utility programs, or by using Toolkit APIs built into the programs that use kernels. 



What is a GMAT script? 

A GMAT script is a text file that is interpreted by GMAT. It has the commands and settings that set up the spacecraft, the force models, any celestial bodies involved, the propagators, burns, the plots, and the visualization and animation. The final part of the script starts the mission sequence. 
Through GMAT’s MATLAB interface, which needs to be correctly configured during installation, MATLAB functions can be called directly from the script. If you do not have MATLAB, and do not have an academic or commercial license, there is an inexpensive home use license available. 
There is another interface in GMAT to Python. This interface also needs to be correctly configured during GMAT installation in order for scripts to access Python functions. 

Resolving some Additional Warnings

While looking at the output of GMAT processing the script, I noticed some warnings about a missing file called aura.3ds

Interpreting scripts from the file.
***** file: /Users/w5nyv/Documents/MATLAB/GMAT/spice_nrho.script
*** WARNING *** The model file ‘C:\Program Files (x86)\GMAT\R2015a\data\vehicle\models\aura.3ds’ does not exist for the spacecraft ‘Gateway’
*** WARNING *** “EarthTideModel” field of GravityField is deprecated and will be removed from a future build; please use “TideModel” instead.
Successfully interpreted the script

I found a GMAT vehicle file called aura.3ds at https://github.com/haisamido/GMAT/blob/master/gmat/application/data/vehicle/models/aura.3ds

It was placed in the GMAT directory and the script updated to point to this file. The warning went away. In the animation, Gateway now has a visual model instead of being represented by a point. 
Getting a gateway.3ds model would be another step forward. A search at https://nasa3d.arc.nasa.gov/models did not turn up anything for Lunar Gateway. Maybe we could make one based on what we know about the spacecraft. 

![][Screenshot2020-04-17163925]

What do we get by running the script?

We get a mathematical model that defines a series of values for the position of a spacecraft over time. We get a configurable visualization that can be converted into an animation. 


----

Here are four animations from GMAT of this model, showing just a few of the different views possible. 

Gateway 1
https://www.youtube.com/watch?v=MLgsnJ2Dqf4

Gateway 2
https://www.youtube.com/watch?v=S6wCfD7nRS0

Gateway 3
https://www.youtube.com/watch?v=4C4TplMzw3o

Gateway 4
https://www.youtube.com/watch?v=3HioDfaDup0

We also get two-dimensional graphs and their associated data files. Exports can be done by right-clicking on the graph and selecting Exporting Data or Save Image.
There does not yet appear to be a native export of the animations, but you can produce them using third party software.

For the Gateway script we are starting with, the following graphs are produced. 

First, there is a Date vs. Gateway Altitude above Luna is the graph with the single red line. The Earth’s Moon is called Luna in GMAT. 

![][PlotImage]




Second, there is a graph of Date vs. a variety of data.

![][Screenshot2020-04-14082513]


----

Going through each of the plotted data, we see that the first three are:

Gateway.Luna_Earth.X
Gateway.Luna_Earth.Y
Gateway.Luna_Earth.Z

The coordinate system is centered on Luna. X is red, Y is green, Z is blue.

![][Screenshot2020-04-13192437]

![][Screenshot2020-04-13192532]

The orbit of Gateway is roughly coplanar in Y and Z. 


----

Below is a detail of the plot for Gateway.Luna_Earth.X, Gateway.Luna_Earth.Y, and Gateway.Luna_Earth.Z. 
When we zoom in at this level of detail we can see a fourth line. This is Gateway.Luna.RadPer. 
(From top to bottom, at the graph origin, we have Y, RadPer, X, and Z. The software could use symbols on the graphs to make it more accessible to colorblind people or black and white prints. (Improving accessibility might be a good opportunity for someone with the right skills to contribute back to the GMAT codebase.)


![][Screenshot2020-04-14082910]

With the coordinate system centered at Luna, one can see the three components of Gateway position (X, Y, and Z) change over time as it traverses the near-rectilinear halo orbit (NHRO). 
As expected of a highly-elliptical orbit, Gateway has a higher velocity when close to to Luna and a slower velocity when farthest away from Luna. 
As expected given the shape of the orbit, the change in Z is larger than the change in X and Y. 
The light blue line (the second line down, right above the zero line) might be mistaken for a horizontal marker, but it is a charted variable called Gateway.Luna.RadPer. 
Per means peri, which is the prefix indicating the point of an orbit closest to the central body. Perihelion is the point of an orbit closest to the sun, if the body being orbited is the sun. Perigee is the point of an orbit closest to the Earth, if the body being orbited is the Earth. In this case, we have a Perilune, the point at which a spacecraft in lunar orbit is closest to the Moon (Luna).
The purple (upper) line in the graph is Gateway.Luna.RadApo. Here’s a detail image. Apo is short for Apolune, the point at which a spacecraft in lunar orbit is farthest away from the Moon (Luna). 

![][Screenshot2020-04-17142248]

 



The final two variables are at the very top of the graph. The assigned colors are brown and yellow. The yellow smooth line is Earth.Luna_Earth.X and the brown scalloped line is Gateway.Earth.RMAG. Below is a detail featuring the two lines.

![][Screenshot2020-04-17165218]

Earth.Luna_Earth.X is interpreted to be the X axis component of the distance from Earth to Luna.
Gateway.Earth.RMAG is interpreted to be the orbital radius from Earth to Gateway. 

How Does GMAT Help AREx?

GMAT helps the AREx project by providing a model of the position of Gateway. Visualizations from a variety of frames of references are possible. 
The challenge for antenna design in space and on the ground is much more easily communicated with the animations, plots, and data. 
For ground stations, it’s clear from the animations that one cannot simply point a 10 GHz dish receiver or 5 GHz transmitter at the moon and expect to reliably communicate with Lunar Gateway. The orbit takes the spacecraft far enough away from the Moon that one must track Gateway separately from the Moon. The calculations and visualizations from GMAT clearly show this. 
Fortunately, if the position is known with standard algorithms, and if these data files are regularly distributed, the spacecraft can be tracked from the ground. 
For payloads deployments where the antenna cannot be pointed, we have to be able to handle the field of view that results from the orbit. AREx anticipates fixed antenna deployments, especially in early missions, in order to reduce complexity and comply with the schedule. 
In general, a fixed antenna means reduced gain compared to a directional pointed antenna. Antenna arrays and other techniques are being actively discussed, but the link budgets will have to account for a reduction in gain due to fixed antennas. 
GMAT can help determine exactly how wide a fixed antenna or array of antennas must be in order to close a reasonable link to Earth. 
In order to add additional variables or useful behavior we next learned how to edit the script. In the visualization, the Gateway spacecraft appears to track the sun. The second edition of the script added Gateway as a frame of reference.

Adding Gateway as a Frame of Reference for Azimuth and Elevation Data

Achim added to the script in order to allow us to extract short and long term azimuth and elevation data. Here is a description of this work. This second revision of the script is in Appendix B.

We assume that the gateway orientation is fixed with its primary axis strictly pointing to the sun. We set up an additional object reference coordinate system with origin Gateway with an axis to the Sun. 

From this point of view, Luna and Earth are orbiting the Gateway. From this coordinate system in X, Y, and Z, we can derive azimuth and elevation. We assume the Sun is along the direction of the X axis.

Azimuth = arctan(y/x) (attention to four quadrants of arctan)
Elevation = arccos(z/dis) with dis=sqrt(x^2+y^2+z^2)












Azimuth, elevation, and distance were added to the plotted charts. However, the first revision of the code produced zeros in the plotted charts. See below. 

![][EarthGatewayFixed]

When azimuth, elevation, and distance data were explicitly output, it was noticed that the final value was reasonable, but all preceding values were zero. 
When variables are declared and maths written outside the kernel, the author can no longer assume that the framework will handle bookkeeping and presentation. 
A loop that stopped at reasonable time intervals and explicit recording of the values for azimuth, elevation, and distance was written and tested. 
Relevant code snippet below. Edited script in Appendix C. 


%----------------------------------------
%---------- Mission Sequence
%----------------------------------------

BeginMissionSequence;

days = 7 % number of days in simulation
step = 360 % step size in seconds
totalDuration = days*24*60^2 % total duration of simulation for loop
nSteps = totalDuration / step % number of times we divide up the duration of the loop


% Report Azimuth, Elevation, and Distance values 
% every 60 minutes for length of the simulation
For i=1:nSteps

%Propagate DefaultProp(Gateway) {Gateway.ElapsedDays = 3}; % original statement for reference
Propagate DefaultProp(Gateway) {Gateway.ElapsedSecs = step};


	AZ = atan2(Earth.GatewayFixed.Y,Earth.GatewayFixed.X)
	DIS = sqrt(Earth.GatewayFixed.X^2+Earth.GatewayFixed.Y^2+Earth.GatewayFixed.Z^2)
	EL = acos(Earth.GatewayFixed.Z/DIS)
	%GMAT DIS = Earth.GatewayFixed.Z+10;   % may not be needed - check with Achim AI:
	Report AzEl AZ EL DIS

	
EndFor
	


----

The report function saves the AZ, EL, and DIS values to a text file called AzEl. 

This file was opened in Microsoft Excel and examined. Plots of the values, from Excel, are below. 

![][AZ-plot]

![][EL-plot]

![][DIS-plot]



Variable
30-day Maximum
30-day Minimum
Azimuth (AZ)
3.14 radians
-3.14 radians
Elevation (EL)
1.66 radians
1.32 radians
Distance (DIS)
426200 kilometers
360425 kilometers

Over 30 days, Azimuth takes all values within 6.28 radians (360 degrees), Elevation varies by 0.34 radians or 19 degrees, and Distance varies from 426200 kilometers to 360425 kilometers.





Three animations were produced to show this new frame of reference. 

Gateway 5
https://www.youtube.com/watch?v=IxPD9s1QBBw

Here, we are in a fixed position above Gateway, looking at Earth and Luna as they appear to orbit around Gateway. Gateway points at Sun, therefore Sun appears fixed.

Gateway 6
https://www.youtube.com/watch?v=0W4aEgUq0kg

Here, we are in a fixed position above Earth, at origin, looking at Gateway move around in the sky.

Gateway 7
https://www.youtube.com/watch?v=-IvU81WD3Pg

Here, we are in a fixed position above Gateway, looking at Earth and Luna as they appear to orbit around Gateway. Gateway points at Sun, therefore Sun appears fixed. We are closer to Gateway than in Gateway 5. 
These animations help show what field of view the Gateway antenna design must cover from this orbit.

Next: 
1. Find or make a model of the Lunar Gateway and other spacecraft that AREx is interested in modeling, so that the spacecraft in the visualization and animation looks as anticipated. 
2. See about plotting the custom equations for AZ, EL, and DIS within GMAT rather than saving to a file and opening these results up in a second program like Microsoft Excel. 


Michelle Thompson W5NYV@arrl.net
Michael Easton KK6OOZ
Achim Vollhardt DH2VA


[Screenshot2020-04-17163925]: Screenshot2020-04-17163925.png width=1494px height=1136px

[PlotImage]: PlotImage.png width=2351px height=663px

[Screenshot2020-04-14082513]: Screenshot2020-04-14082513.png

[Screenshot2020-04-13192437]: Screenshot2020-04-13192437.png width=993px height=835px

[Screenshot2020-04-13192532]: Screenshot2020-04-13192532.png width=936px height=851px

[Screenshot2020-04-14082910]: Screenshot2020-04-14082910.png

[Screenshot2020-04-17142248]: Screenshot2020-04-17142248.png width=319px height=544px

[Screenshot2020-04-17165218]: Screenshot2020-04-17165218.png width=786px height=314px

[EarthGatewayFixed]: EarthGatewayFixed.png width=2594px height=1266px

[AZ-plot]: AZ-plot.png width=495px height=373px

[EL-plot]: EL-plot.png width=449px height=375px

[DIS-plot]: DIS-plot.png width=490px height=374px