# win10 installer yappari
 latest installer for win 10

![image](https://user-images.githubusercontent.com/80171481/233996948-d8085a78-65ee-47cc-b291-95b75b5f60d1.png)

__11th of may 2023__

# YAPPARI 
YAPPARI stands for _**Y**et **A**nother **P**rogram for **An**alysis and **R**esearch in **I**mpedance_.
This file is referenced as [http://dx.doi.org/10.13140/RG.2.2.15160.83200](http://dx.doi.org/10.13140/RG.2.2.15160.83200)

If you are using Windows 10, you can download and install YAPPARI from this page (Select **Code** then **Download zip**). A version compiled in Labview 2018 can also be found in one of my repositories [here](https://github.com/nitad54448/yappari). The difference between them is that on some processors without SSE2 instructions set, only the LV2022 will work.

YAPPARI is designed to simulate or fit the impedance spectrum of simple circuits and is primarily intended for educational purposes. This help file provides a brief introduction to the program. You are welcome to customize the help_YAPPARI.pdf for your own use or for other users. If you wish to contribute to the file feel free to modify it here.

## Main window

### Read Data
This command selects the file format where to read the data. As of now there are three possibilities :

a) __3 cols tabs__ reads a three-column ASCII file, which should be separated by tabs and contain frequency in Hz, Zr, and Zi. It is important to note that for French users (and some others), the separator value should be a dot “.” and not a comma “,”. If the reading is successful, the data will be plotted.

b) __Z view txt__ reads a file saved in a Z view format by the MFLI/MFIA lock in amplifier. I have no ideea if this is the valid Zview file format

c) __MFLI csv__ reads the impedance data from an IMPS file saved by MFIA or MFLI

For the MFLI/MFIA users : only the first data set found in the txt or csv file will be read. Remember to save the data you want to analyze, Auto Save will append data to file.

### Fit
This command is used to fit a set of parameters given that there are some data and a valid circuit (i.e., there are parameters to fit on the right side of the window). The user can select which parameters to fit and it is recommended to start with a few parameters first, ensuring that the initial values are close to the expected values. The simulated spectrum will be updated with every change in the parameters, and the user can perform manual adjustments as necessary.

The fitting can be performed using different methods, which are discussed below, although there is not much difference in the output of these methods. The fitting process involves up to 5000 cycles, and multiple iterations may be necessary, particularly if the initial values are far from the actual values.

The quality of the fit is evaluated using the R^2 statistical parameter and the chi^2 value. However, the use of the chi^2 value as a statistical parameter is debatable, as discussed in the paper "Dos and don'ts of reduced chi-squared" by [Andrae et al.](https://arxiv.org/abs/1012.3754). The chi^2 value reported here is calculated as (Sum ((Z_obs-Z_calc)^2/Z_calc))/DOF, with each term multiplied by a weight. The degree of freedom (DOF) is considered as Nr_of_points - nr_of_fitted_params. The user can select the weight for the fit.

### Method
This command allows the user to select the method used for nonlinear fitting. There are three methods available: Trusted Region Dog Led algorithm (TRDL), Constrained Levenberg Marquardt, and Levenberg Marquardt. The user can choose any of these methods, and if the model is robust, they should obtain the same results. 

For TRDL and Constrained LM, the fit is constrained to certain intervals. For example, resistors are limited to the range of 1 mOhm to 1 GOhm, capacitors are between 10^-4 and 10^-14, and so on. It is recommended to start with TRDL and then make a final fit with the standard (unconstrained) LM method.

### Weight
This command selects the weight used for fitting, there are four possibilities:
Z
Zr
Zi
Equal

### Action

This regroups a number of small functions that you can select :

#### Save sim
This command allows you to save the simulated data to a file in a specific format. The format is three columns separated by tabs, with frequency in Hz, Zr, and Zi. This is useful for simulating impedance spectra for a given model. 
If no data were previously read, the program will use 100 frequency points ranging from 10 mHz to 1 MHz in log spacing. However, if you read an experimental data file, the simulation of the impedance spectrum will only be made at the frequencies read from your data file. 
It's important to note that if you have both experimental and simulated data, using the Report command might be a better choice since it allows you to see both the calculated and experimental data together.

#### Report
This command generates an HTML report containing information about the model used, the parameters used, the fitted parameters, and their standard deviation. It also includes images of the fit as well as all experimental and calculated data. The report is saved in your temporary directory and automatically opened in a browser. You can use the data in the report to create your own graphs or to check for any discrepancies. If you find any errors in the calculations, please report them so they can be corrected.

#### Clear model
This command will clear the model page (instead of erasing each element one by one).

#### Help
This command will help an (old) pdf file. Better look at this Readme.md file which is updated more regularly.

### Exit
No need for explications on what this command does.

#### Zr, -Zi
This panel shows a Nyquist plot, which is a standard way to visualize impedance data. The scale on the graph will adjust automatically based on the data. However, if you want to manually set a specific range, you can disable the Auto-axis feature by clicking on the graph. Additionally, you have the option to add markers for specific frequencies, and you can even move them around on the graph using the Details panel.

#### Zr, Zi, Z/phi
These panels will show the dependency of impedances (real, imaginary or modulus) as a function of frequency and the differences between the calculated and experimental values (if any).

#### Model
A circuit can be created by the user by selecting element circuits, on this page. 
![image](https://user-images.githubusercontent.com/80171481/233996991-3ec2e642-f2ea-4ee4-a044-d16dc08622cd.png)

Up to ten elements can be added (obviously it is not realistic to fit such a circuit, unless you want to fit an [elephant](https://demonstrations.wolfram.com/FittingAnElephant/). Only the first 18 parameters will be shown (to prevent outrageous fits).
When you click on one of the ten available cases, a new window will appear where you can select the element you want to add. Simply click on the picture of the element you want to add and it will be added to the model. The available circuit elements include resistors, capacitors, inductors, and more complex elements such as constant phase elements or Warburg elements.
![image](https://user-images.githubusercontent.com/80171481/233997021-920d4067-2e55-4161-a401-360aa427cd3f.png)

You can edit the png image files to your liking (just for aesthetics, the calculations are not affected), they are in the subdirectory /models. The size of the png files should be 145*100 pixels.

The elements used now (as of march 2023) are: Resistor, Capacitor, Inductor, CPE, Zarc, simple Randles circuit, Randles with kinetic and diffusion, Warburg (infinite diffusion, equivalent with a CPE with n=0.5 coefficient), Warburg short, Warburg Long, Gerischer; Havriliak-Negami and several compositions of these.
Warburg "infinite" diffusion coefficient s is expressed here as:

![image](https://user-images.githubusercontent.com/80171481/233997055-73e74f9d-ffb1-4766-85ae-ed7fe96ded87.png)
 
The parameters obtained for Warburg in other programs are typically by fitting a CPE with n=0.5, you will get the same result but the Q parameter obtained is :

![image](https://user-images.githubusercontent.com/80171481/233997075-54bd6dbd-a049-43f5-9e3c-dbe903cd2d71.png)

The Warburg "open" describes the impedance of a finite-length diffusion with reflective boundary.  The formula used here is

![image](https://user-images.githubusercontent.com/80171481/233997097-dba594f4-354d-41e2-955c-52fa5cc9e105.png)

The Warburg "short" describes the impedance of a finite-length diffusion with transmissible boundary, with the expression:

![image](https://user-images.githubusercontent.com/80171481/233997133-24bde427-91e9-4ecb-adbf-676232ffa5c4.png)

Some others can be added upon request, if I will have the time and if there is an interest for it.

When you create a circuit using the circuit editor, the circuit is not valid until you have properly connected all the elements together. Once the circuit is valid, a LED labeled "valid" will light up on the model panel, indicating that the circuit is ready for use.

![image](https://user-images.githubusercontent.com/80171481/233997150-328d2100-d091-4def-8a9e-1a78077353d6.png)

In the above figure you may notice the top left indicator, Valid, as OFF. The circuit is not closed so no calculation can be made. Let’s make a valid circuit, as here:

![image](https://user-images.githubusercontent.com/80171481/233997165-bf2358b8-3705-4118-8892-4f261b43344a.png)

When you add a parallel RQ element to the first element of the circuit (a Zarc as element 0), you need to create "electrical contacts" in the next three elements (elements 1, 2, and 3, or 4,5 and 6, or 7, 8 and 9….) for the circuit to be complete. Otherwise, the circuit will be open and no impedance can be calculated. In other words, all the elements of the circuit need to be properly connected for the circuit to be valid and for impedance calculations to be performed.
Once the circuit is valid, you can see a list of all the parameters for each element of the circuit. Each parameter is labeled with a decimal, which indicates which element it belongs to. For example, the first element of the circuit will have parameters labeled as 0.x, the second element as 1.x, and so on.

![image](https://user-images.githubusercontent.com/80171481/233997187-bf156ba5-0d74-4e4b-9a59-ceccc134c46c.png)

You can use the wheel of the mouse to evaluate the change in the output impedance in real time.

For a more complex circuit, like in the following image:

![image](https://user-images.githubusercontent.com/80171481/233997202-b715aff3-015e-46f4-9d4b-f5d141ca7810.png)

On the right side of the screen, you can see names such as 2MR1D, 2MQ2D, 2MN2D, 2MR3D, 2MR4D, 2MR5D, and 2MW6D. The first number, "2", indicates which element case the device is in. The letters "M" and "D" are internal notations that are used by the program to identify the device type, but they are not important for the user. The type of device is listed after the "M" notation, such as "R" for resistor or "W" for Warburg. The numbering of the devices goes from left to right and top to bottom. For example, the first device is a resistor and can be described by the parameter "2MR1D". The second device in the circuit is a Zarc, which is a combination of a constant phase element (CPE, or Q) in parallel with a resistor. This device is described by the parameters "Q2" and "N2". 

Overall, the notation is quite straightforward once you become familiar with the conventions used.

#### Details
In this panel the last values for R2 and chi2 are listed. The values for R2 and chi2 are important metrics for determining the quality of the fit. R2 is the coefficient of determination, which gives an indication of how well the model fits the data. A value of 1 indicates a perfect fit, while lower values indicate poorer fits. Chi2, or the chi-squared statistic, is another measure of how well the model fits the data, but it takes into account the number of data points and parameters in the model. These values, together with standard error for parameters are also printed with the Report command.

You can also show markers at given frequencies listed in the Values table; by Source I mean you mark the calculated frequency or the experimental one. The precision gives the number of digits shown in the graphic. Note that you can drag the text, as well as the marker (albeit there is no use for this later). The markers are listed only in the Nyquist plot.

### Author
This program can be used freely and distributed according to CC-BY-NC-SA.
It was written in Labview and it includes the JKI toolkits for Labview, © 2018, JKI. All rights reserved.

For questions or comments:

Nita DRAGOE
Université Paris-Saclay
ICMMO/SP2M
91400 Orsay
France


A first version was released in june 2021.

