# JSON configuration

Javascript object notation (json) may used to pass parameters to the application in order to configure its behavior.  

The _stanford-stages app_ may be run using __run_stanford_stages.py__ (or one of its wrapper scripts) with a .json configuration file, or by invoking the main method of __inf_narco_app.py__ with either a .json file or json formatted parameters.   See [for the curious](#for_the_curious) for information regarding fields that are supported by _run_stanford_stages.py_ or _inf_narco_app.py_, but not both.

Sample json input is included below and in the repository's code base (see [stanford_stages.json]()).  

A description of the supported key-value pairs is provided below.


## `channel_indices`
   
### Description
 Assigns channel indices corresponding to the central, occipital, ocular, and chin EMG. One or two EEG channels may be assigned
to the central and occipital categories.  Both left and right EOG channels are required for the corresponding occular category, and one channel is required for the chin EMG.  In the event that two channels are presented for an EEG category (central or occipital), a quality metric is calculated
for each channel, and the optimal channel is selected for use.  Channel indices are 0 based and correspond to the channel labels list provided in
the .EDF file header.

 ### Supported keys and values

* `central`: C3 or C4 channel index
* `central3`: C3 channel index
* `central4`:  C4 channel index
* `centrals`: [C3 channel index, C4 channel index]
* `occipital`: O1 or O2 channel index
* `occipital1`: O1 channel index
* `occipital2`: O2 channel index
* `occipitals`: [O1 channel index, O2 channel index]
* `eog_l`: Left EOG channel index
* `eog_r`: Right EOG channel index
* `eogs`: [Left EOG channel index, Right EOG channel index]
* `chin_emg`: EMG channel index
* `show`
   * Description
    Flags for determining which results are output to the console.
   * Supported keys: allowed and [default] values
      * `plot`: true or [false]
      * `hypnodensity`: true or [false]
      * `hypnogram`: true or [false]
      * `diagnosis`: [true] or false

## `save`
### Description
Flags for determining which results are saved to disk.  Files are saved to the same directory 
as the input .edf file.  Save filenames are generated by pairing the .edf file's basename with 
the file extension matching the output type requested.

### Supported keys and values

* `plot`: [true] or false  (.hypnodensity.png)
* `hypnodensity`: [true] or false (.hypnodensity.txt)
* `hypnogram`: [true] or false (.hypnogram.txt)
* `diagnosis`: [true] or false (.diagnosis.txt)

[default] values (file extension used by default when true)

## `filename`
### Description

Alternative filename to use in place of the default naming convention and path location used when saving results.  
### Supported keys
* `plot`: Full path for hypnodensity image file
* `hypnodensity`: Full path for hypnodensity output text file
* `hypnogram`: Full path for hypnogram output text file
* `diagnosis`: Full path for diagnosis output text file


# Example .json
```json
{
    "channel_labels": {
        "central3": "EEG C3-A2",
        "central4": "EEG C4-A1",
        "chin_emg": "EMG Chin",
        "eog_l": "EOG LOC-A2",
        "eog_r": "EOG ROC-A2",
        "occipital1": "EEG O1-A2",
        "occipital2": "EEG O2-A1"
    },
    "edf_filename": "C:/Data/ml/CHP040.edf",
    "inf_config": {
        "atonce": "1000",
        "hypnodensity_model_dir": "F:/ml/ac/",
        "hypnodensity_scale_path": "F:/ml/scaling",
        "lights_filename": "",
        "lights_off": "",
        "lights_on": "",
        "models_used": [
            "ac_rh_ls_lstm_01",
            "ac_rh_ls_lstm_02",
            "ac_rh_ls_lstm_03",
            "ac_rh_ls_lstm_04",
            "ac_rh_ls_lstm_05",
            "ac_rh_ls_lstm_06",
            "ac_rh_ls_lstm_07",
            "ac_rh_ls_lstm_08",
            "ac_rh_ls_lstm_09",
            "ac_rh_ls_lstm_10",
            "ac_rh_ls_lstm_11",
            "ac_rh_ls_lstm_12",
            "ac_rh_ls_lstm_13",
            "ac_rh_ls_lstm_14",
            "ac_rh_ls_lstm_15",
            "ac_rh_ls_lstm_16"
        ],
        "narco_classifier_path": "C:/Data/OAK/narco_test_updated_b_gp_models_1000",
        "psg_noise_filename": "F:/ml/noiseM.mat",
        "segsize": "60"
    },
    "output_path": "C:/Data/ml/results",
    "save": {
        "diagnosis": "true",
        "encoding": "true",
        "hypnodensity": "false",
        "hypnogram": "false",
        "hypnogram_30_sec": "false",
        "plot": "false"
    },
    "show": {
        "diagnosis": true,
        "hypnodensity": false,
        "hypnogram": false,
        "hypnogram_30_sec": "true",
        "plot": "false"
    }
}

```     
      
# For the curious

The _stanford-stages app_ may be run using __run_stanford_stages.py__ (or one of its wrapper scripts) with a .json configuration file, or by invoking the main method of the ___inf_narco_app___ module with a .json file or a json formatted parameters. 

The .json parameters are slightly different between __run_stanford_stages.py__ and __inf_narco_app.py__.  The __run_stanford_stages.py__ code accepts handles additional .json arguments, which it then
parses and converts to a compatible form for use with __inf_narco_app.py__, which it calls directly.  

An example of these differenes are the `channel_labels` field which includes the channel names listed in the .edf file (e.g. 'C3-M1'), which _run_stanford_stages_ will examine to find the 
corresponding channel indices (e.g. 0) and place in the `channel_indices` field used by _inf_narco_app_.

      