# win10-installer-yappari-4.2 #
**latest installer**

YAPPARI stands for Yet Another Program for Analysis and Research in Impedance.
This file is referenced as http://dx.doi.org/10.13140/RG.2.2.15160.83200

If you are using Windows 10, you can download and install YAPPARI from this page or if you look for a more advanced, multiple dataset, I made a version called Yappari 5, available here on Github in one of my repositories.

YAPPARI is designed to simulate or fit the impedance spectrum of simple circuits and is primarily intended for educational purposes. This help file provides a brief introduction to the program. You are welcome to contribute to the file, you can send it to me or fork it on Github.

## Main window ##

### Open File ###
This command reads a three-column ASCII file, which should be separated by tabs and contain frequency in Hz, Zr, and Zi. It is important to note that for French users (and some others), the separator value should be a dot “.” and not a comma “,”. If the reading is successful, the data will be plotted.

### Fit ###
This command is used to fit a set of parameters given that there are some data and a valid circuit (i.e., there are parameters to fit on the right side of the window). The user can select which parameters to fit and it is recommended to start with a few parameters first, ensuring that the initial values are close to the expected values. The simulated spectrum will be updated with every change in the parameters, and the user can perform manual adjustments as necessary.

The fitting can be performed using different methods, which are discussed below, although there is not much difference in the output of these methods. The fitting process involves up to 5000 cycles, and multiple iterations may be necessary, particularly if the initial values are far from the actual values.

The quality of the fit is evaluated using the R^2 statistical parameter and the chi^2 value. However, the use of the chi^2 value as a statistical parameter is debatable, as discussed in the paper "Dos and don'ts of reduced chi-squared" by Andrae et al. (https://arxiv.org/abs/1012.3754). The chi^2 value reported here is calculated as (Sum ((Z_obs-Z_calc)^2/Z_calc))/DOF, with each term multiplied by a weight. The degree of freedom (DOF) is considered as Nr_of_points - nr_of_fitted_params. The user can select the weight for the fit.

### Method ###
This command allows the user to select the method used for nonlinear fitting. There are three methods available: Trusted Region Dog Led algorithm (TRDL), Constrained Levenberg Marquardt, and Levenberg Marquardt. The user can choose any of these methods, and if the model is robust, they should obtain the same results. 

For TRDL and Constrained LM, the fit is constrained to certain intervals. For example, resistors are limited to the range of 1 mOhm to 1 GOhm, capacitors are between 10^-4 and 10^-14, and so on. It is recommended to start with TRDL and then make a final fit with the standard (unconstrained) LM method.

### Report ###
This command generates an HTML report containing information about the model used, the parameters used, the fitted parameters, and their standard deviation. It also includes images of the fit as well as all experimental and calculated data. The report is saved in your temporary directory and automatically opened in a browser. You can use the data in the report to create your own graphs or to check for any discrepancies. If you find any errors in the calculations, please report them so they can be corrected.

### Save sim ###
This command allows you to save the simulated data to a file in a specific format. The format is three columns separated by tabs, with frequency in Hz, Zr, and Zi. This is useful for simulating impedance spectra for a given model. 
If no data were previously read, the program will use 100 frequency points ranging from 10 mHz to 1 MHz in log spacing. However, if you read an experimental data file, the simulation of the impedance spectrum will only be made at the frequencies read from your data file. 
It's important to note that if you have both experimental and simulated data, using the Report command might be a better choice since it allows you to see both the calculated and experimental data together.

### Exit ###
No need for explications on what this command does.

## Panels ##
### Zr, -Zi ###
This panel shows a Nyquist plot, which is a standard way to visualize impedance data. The scale on the graph will adjust automatically based on the data. However, if you want to manually set a specific range, you can disable the Auto-axis feature by clicking on the graph. Additionally, you have the option to add markers for specific frequencies, and you can even move them around on the graph using the Details panel.

### Zr, Zi, Z/phi ###
These panels will show the dependency of impedances (real, imaginary or modulus) as a function of frequency and the differences between the calculated and experimental values (if any).

### Model ###
A circuit can be created by the user by selecting element circuits.
Up to ten elements can be added (obviously it is not realistic to fit such a circuit, unless you want to fit a crocodile). Only the first 18 parameters will be shown (to prevent outrageous fits).
When you click on one of the ten available cases, a new window will appear where you can select the element you want to add. Simply click on the picture of the element you want to add and it will be added to the model. The available circuit elements include resistors, capacitors, inductors, and more complex elements such as constant phase elements or Warburg elements.

You can edit the png image files to your liking (just for aesthetics, the calculations are not affected), they are in the subdirectory __/models__. The size of the png files should be 145x100 pixels.

The elements used now (as of march 2023) are: Resistor, Capacitor, Inductor, CPE, Zarc, simple Randles circuit, Randles with kinetic and diffusion, Warburg (infinite diffusion, equivalent with a CPE with n=0.5 coefficient), Warburg short, Warburg Long, Gerischer; Havriliak-Negami and several compositions of these.

Warburg "infinite" diffusion coefficient s is expressed here as:

$(Zw= sw^-1/2 -j w^-1/2)$

The parameters obtained for Warburg in other programs are typically by fitting a CPE with n=0.5, you will get the same result but the Q parameter obtained is
 

The Warburg "open" describes the impedance of a finite-length diffusion with reflective boundary.  The formula used here is
 

The Warburg "short" describes the impedance of a finite-length diffusion with transmissible boundary, with the expression:
 

Some others can be added upon request, if I will have the time and if there is an interest for it.

When you create a circuit using the circuit editor, the circuit is not valid until you have properly connected all the elements together. Once the circuit is valid, a LED labeled "valid" will light up on the model panel, indicating that the circuit is ready for use. 

Once the circuit is valid, you can see a list of all the parameters for each element of the circuit. Each parameter is labeled with a decimal, which indicates which element it belongs to. For example, the first element of the circuit will have parameters labeled as 0.x, the second element as 1.x, and so on.

When you add a parallel RQ element to the first element of the circuit (a Zarc as element 0), you need to create "electrical contacts" in the next three elements (elements 1, 2, and 3, or 4,5 and 6, or 7, 8 and 9….) for the circuit to be complete. 
Otherwise, the circuit will be open and no impedance can be calculated. In other words, all the elements of the circuit need to be properly connected for the circuit to be valid and for impedance calculations to be performed.

 

In the above figure you may notice the top left indicator, Valid, as OFF. The circuit is not closed so no calculation can be made. Let’s make a valid circuit, as here: 

Now, as the circuit is valid, with a Zarc in element 0 position, three parameters will appear in the tab of the right side of the panel: 0ZARR, 0ZARQ and 0ZARN; the names are rather self-explaining for the parameters describing a Zarc located in the position 0 of the circuit, with the three parameters describing a parallel RQ. You can use a RQ element and fix the N to 1 to obtain the equivalent RC circuit. The equivalent capacitance for a RQ circuit is C=((RQ)^1/n)/R.

The program will calculate the impedance spectra of a circuit with the values listed in the right side tab, for this case we get 
 
You can use the wheel of the mouse to evaluate the change in the output impedance in real time.

For a more complex circuit, like in the following image:

 
On the right side of the screen, you can see names such as 2MR1D, 2MQ2D, 2MN2D, 2MR3D, 2MR4D, 2MR5D, and 2MW6D. The first number, "2", indicates which element case the device is in. 

The letters "M" and "D" are internal notations that are used by the program to identify the device type, but they are not important for the user. The type of device is listed after the "M" notation, such as "R" for resistor or "W" for Warburg. 

The numbering of the devices goes from left to right and top to bottom. For example, the first device is a resistor and can be described by the parameter "2MR1D". The second device in the circuit is a Zarc, which is a combination of a constant phase element (CPE, or Q) in parallel with a resistor. This device is described by the parameters "Q2" and "N2". 

Overall, the notation is quite straightforward once you become familiar with the conventions used.

Save sim
The "Save Sim" command allows you to save the impedance spectrum data calculated by the software in a file. This can be useful if you want to use the data for further analysis or visualization.
In addition to the "Save Sim" command, there is also a report page that generates a HTML file. This report includes not only the calculated data but also the reports of the fitted values. This means that you can see the parameter values that were used to generate the impedance spectrum, as well as any statistics or other relevant information about the fitting process. The report can be a helpful tool for understanding the quality of the fit and for identifying any areas where improvements could be made.

Details
In this panel the last values for R2 and chi2 are listed. The values for R2 and chi2 are important metrics for determining the quality of the fit. R2 is the coefficient of determination, which gives an indication of how well the model fits the data. A value of 1 indicates a perfect fit, while lower values indicate poorer fits. Chi2, or the chi-squared statistic, is another measure of how well the model fits the data, but it takes into account the number of data points and parameters in the model. These values, together with standard error for parameters are also printed with the Report command. 

You can also show markers at given frequencies listed in the Values table; by Source I mean you mark the calculated frequency or the experimental one. The precision gives the number of digits shown in the graphic. Note that you can drag the text, as well as the marker (albeit there is no use for this later). The markers are listed only in the Nyquist plot.

About
Brief help listing the version of the program. The button “more” will open this pdf file.

Author
This program can be used freely and distributed according to CC-BY-NC-SA.
It was written in Labview and it includes the JKI toolkits for Labview, © 2018, JKI. All rights reserved.

For questions or comments:

Nita DRAGOE
Université Paris-Saclay
ICMMO/SP2M
91400 Orsay
France


A first version was released in june 2021.
