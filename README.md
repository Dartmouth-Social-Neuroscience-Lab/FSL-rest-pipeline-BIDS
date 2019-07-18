# FSL-rest-pipeline-BIDS
FSL pipeline to preprocess fMRI rest period data and extract ROI time courses. Set up to work with BIDS format data. Follow steps below carefully.

BEFORE BEGINNING PIPELINE
1. Run setup/transfer_fMRI_data.sh for each subject:
    * Transfers data from BIDS folder on Rolando server to study folder on Discovery, and swaps out SIDs in filenames with subject numbers
    * You will be prompted for your Rolando password, which is by default Change!t
    * Script handles one subject at a time
2. Run setup/unzip_files.sh
    * Unzips anatomical & functional niftis so you can open them in SPM (step 3)
    * Script can handle as many subjects as you specify
3. Reorient anatomical & functional niftis in SPM
    * Follow this guide: https://docs.google.com/document/d/1jp0XflzEsHiiv8q96jHW16ihkIazlIETKaaRH_hq72E/edit?usp=sharing
4. Run setup/zip_files.sh
    * Zips anatomical & functional niftis again for FSL pipeline
    * Script can handle as many subjects as you specify


PIPELINE STEPS
1. Run 1_strip_skulls.sh
    * Strips skull/non-brain matter from anatomical image
    * Adjust the FIT parameter which controls how much matter gets stripped; you may want to record this value somewhere
    * After running script, inspect each subject's anatomical image in fslview to ensure no loss of PFC (too much stripped) or excess dura around brain (too little stripped), then adjust FIT parameter as needed and re-run script until you like the results
     * It may be easiest to first run all subjects with a ballpark FIT parameter (~0.3), then tweak it for each subject individually as needed
    * To run script faster, submit as job using jobs/strip_skulls.pbs
      * Discovery can handle all subjects at once
2. Run 2_segment_tissue.sh
3. Run 3_preproc.sh
4. Run 4_make_nuisance.sh
5. Run 5_clean_nuisance.sh
6. Run 6_extract_roi_timecourses.sh
