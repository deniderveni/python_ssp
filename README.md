# python_ssp
WIP. Run test.py to get an SSP


test.py sets inital values for all the variables. It then runs the following programs:
  - stpars - This code calculates the Teff, logg and Z components of several stars along an isochrone. The only isochrone I have atm is Padova age 10
  - interpall - This creates interpolated spectra for all the stars contained in the stpars output file
  - SSP_model - This combines all the spectra together to create an SSP.

:shipit:

The contained codes and their functions:

## stpars.py

stpars.py contains 3 different functions: stpars(), gettracks() and set_stpars_filename().

**stpars(n_ms, n_rg, feh, afe, age, logg_cn = 3, fig = False, iso = 'DARTMOUTH')**
  - stpars uses the following required inputs:
    - n_ms = number of main sequence stars
    - n_rg = number of red giant stars
    - feh = iron abundance [Fe/H]
    - afe = [alpha/Fe]
    - age = age of the population (in Gyrs)
    
  - stpars uses the following additional inputs:
    - logg_cn = stars with logg <= logg_cn
    - fig = logical, whether you want the isochrone plotted or not
    - iso = isochrone model used, currently supports DARTMOUTH or PADOVA

stpars finds the appropriate isochrone for the desired inputs and calculates the Teff and logg of the n_ms stars on the main sequence and n_rg on the red giant branch. It outputs a table of the Teff's, logg's, masses, Hband logL's and their respective phases in a .dat file found in the folder Stellar_pars.

**gettracks(feh, afe, age, iso = 'DARTMOUTH')**

gettracks uses the previously defined parameters to find the correct isochrone. It outputs the file path of the isochrone selected.

**set_stpars_filename(n_ms, n_rg, feh, afe, age, logg_cn = 3)**

This outputs the filename and path for the parameter file given the previously defined inputs.


## interp.py

interp.py contains two functions: interpall() and interpolate()

**interpolate(Teff_new, logg_new, Z_new)**
  - interpolate requires the following required inputs
      - Teff_new = the effective temperature of the new star n Kelvins
      - logg_new = the log(g) of the new star
      - Z = the metallicity of the new star in Z
  
This is the main interpolator. Given the above parameters it will interpolate a spectra from the data in the irtf folder. The spectra will be stored in Stellar_Spectra/ along with a plot of the spectra.
  
**interpall(n_ms, n_rg, feh, afe, age, Z)**

interpall's inputs have already been defined.

interpall uses interpolate() to calculate spectra for all stars that are in the paramater file created in _stpars.py_.

## retrieve_irtf.py

retrieve_irtf.py contains several functions: param_retrieve(), get_spectra() and set_spectra_name()

**param_retrieve()**

param_retrieve() requires no inputs and simply converts the _irtf_param.txt_ file to a more usable format.

**get_spectra(ID)**

  - get_spectra requires only one input
    - ID = the ID of the irtf file you require (ie 'IRL012')

This function retrieves an irtf .fits file for a specific ID into a usable format and returns it.

**set_spectra_name(Teff, logg, Z)**

This function takes the previously defined paramaters and outputs a filename for the interpolated spectra in _interp.py_

## SSP_model.py

SSP_model is an apdated version of https://github.com/marinatrevisan/SynSSP_PFANTnew.
