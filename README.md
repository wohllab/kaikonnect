# Kaikonnect
A wrapper to serve as an interconnect between [Kaiko](https://github.com/PNNL-Comp-Mass-Spec/Kaiko) and [TagGraph](https://sourceforge.net/projects/taggraph/) for peptide open modification searching.
Thanks to the structures of Kaiko and TagGraph, Kaikonnect is capable of taking in several .mzML or Thermo .raw simultaneously for joint analysis.

## Prerequisites:
A linux machine with:
1. Docker
2. Python 2.7
3. optparse for python 2.7 (this should be installed automatically via pip during execution if not found)

## Installation and Execution:

### First, clone the repo:
``` git clone https://github.com/wohllab/kaikonnect ```

and change into the new directory...

``` cd kaikonnect/ ```

### Second, docker builds and downloads:
This interconnect will bind [Kaiko](https://github.com/PNNL-Comp-Mass-Spec/Kaiko) (the PNNL variant of DeepNovo) and the Elias lab's [TagGraph](https://sourceforge.net/projects/taggraph/) softwares together.
Accordingly, both will need to be downloaded.  For Kaiko, we will also download the default model from MassIVE over FTP.  If you are attempting to connect from within a controlled environment, you may find it necessary to stage the model manually.  Please refer to the ``` tag_grab.sh ``` script for the relevant URLs.

To download all necessary components, and build the docker images, run:
``` sh tag_grab.sh ```

When prompted, enter root password to elevate access for "sudo" commands.  This is necessary to build docker images, unless your user has been granted special permissions.

### Third, let's execute Kaikonnect!
If using Thermo instrument data, Kaikonnect will use the available pwiz docker image to convert the raw data to mzML through msconvert under wine.  Otherwise, we can take in mzML files directly.  In all cases, users are required to provide the mzML_folder argument.  This argument will either be used as the data input, or as the mzML file output after conversion.

As Kaikonnect handles the execution of several docker containers, it is common that root privileges are required.
It is easiest, therefore, to run the whole python script under root or through sudo.  If this is not done, and the --no_sudo flag is not thrown, a warning message will be displayed which a user can press <enter> through to ignore.  Users in this situation may find themselves prompted for sudo passwords multiple times throughout the execution of Kaikonnect, where progress hangs until a sudo password is supplied.

An example command line, with the provided data **(staged when running tag_grab.sh)**:

``` sudo python kaikonnect.py --mzML_folder example/mzml_files/ --output_folder test_output/ --fasta example/human_uniprot_12092014_crap.fasta ```

or more simply, by executing...
``` sudo sh test_script.sh```

For additional configuration options, run:
```
$ python kaikonnect.py --help
Usage: kaikonnect.py [options]

Options:
  -h, --help            show this help message and exit
  -s, --no_sudo
  -r RAW_FOLDER, --raw_folder=RAW_FOLDER
  -m MZML_FOLDER, --mzML_folder=MZML_FOLDER
  -o OUTPUT_FOLDER, --output_folder=OUTPUT_FOLDER
  --kaiko_topk=KAIKO_TOPK
  --kaiko_beam_size=KAIKO_BEAM_SIZE
  -f FASTA, --fasta=FASTA
  --tg_per_fraction
  --tg_FDR_cutoff=TG_FDRCUTOFF
  --tg_logEM_cutoff=TG_LOGEMCUTOFF
  --tg_Display_Protein_Num=TG_DISPLAYPROTEINNUM
  --tg_ExperimentName=TG_EXPERIMENTNAME
  --tg_ppmstd=TG_PPMSTD
  --tg_modtolerance=TG_MODTOLERANCE
  --tg_maxcounts=TG_MAXCOUNTS
  --tg_modmaxcounts=TG_MODMAXCOUNTS
  --tg_EMinitIterations=TG_EMINITITERATIONS
  --tg_EMmaxIterations=TG_EMMAXITERATIONS
 ```
 Please refer to [Kaiko](https://github.com/PNNL-Comp-Mass-Spec/Kaiko) and [TagGraph](https://sourceforge.net/projects/taggraph/) for details about the argument implementations.
