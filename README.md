# A prototype for Global Muon Tracks in ALICE/O2

This repository contains a prototype for global muon tracking in ALICE/O2, including tools to study matching & tracking.

`matcher.sh` automates the process. See `matcher.sh --help`.

### Quick example

Ensure your system has `O2/Latest` and `AliPhysics/latest` available to `alienv`. OCDB files must be found according to the instructions bellow. Then run

    matcher.sh --genMCH --genMFT --match --check -n 5 --nmuons 4 --npions 20 -o matching_dir

Where `matching_dir` is a destination directory (usually non-existant) on which the generation, reconstruction, matching and checks will be executed. The example above consists of 5 events with 4 muons and 20 pions (2 mu-, 2 mu+, 10 pi+, 10 pi-).

## Breaking into steps

### 1. Generate MCH

MCH tracks are created by simulations using aliroot. A configurable generator is found on `gen`. Convertion of MCH tracks to a transitional format compatible with O2 is done via macro `ConvertMCHESDTracks.C`. Use option `--genMCH` as in

    matcher.sh --genMCH -n 5 --nmuons 4 --npions 20 -o matching_dir

### 2. Generate MFT Tracks using the same kinematics

MFT tracks are generated by the same kinematics file using `o2-sim`. Use option `--genMFT` as in

    matcher.sh --genMFT -o matching_dir

### 3. MCH-MFT Track Matching

MCH-MFT track matching and Global Muon Tracking is performed with class `MuonMatching` as used on `runMatching.C`.  Use option `--match` as in

    matcher.sh --match -o matching_dir

### 4. Global muon tracks checks

Macro `GlobalMuonChecks.C` can be used to check tracking. Histograms saved to `GlobalMuonChecks.root`. Use option `--check` as in

    matcher.sh --check -o matching_dir

If all steps completed sucessfully, you may find some histograms on `GlobalMuonChecks.root`.

For more usage options see `matcher.sh --help`.


## Basic Configuration

* OCDBs from https://gitlab.cern.ch/alisw/AliRootOCDB.git at `~/alice/OCDB`
  * `git clone https://gitlab.cern.ch/alisw/AliRootOCDB.git $HOME/alice/OCDB`
* Build aliroot & O2:
  * `cd ~/alice` # or your alice software folder
  * `aliBuild init AliPhysics@master`
  * `aliBuild build AliPhysics --defaults user-next-root6`
  * `aliBuild init O2@dev --defaults o2`
  * `aliBuild build O2 --defaults o2`

### Docker?

This code has been developed and tested on [Alidocklite](https://github.com/rpezzi/alidocklite). If you are having problems with O2 on your system or Alidock, you may try it.
