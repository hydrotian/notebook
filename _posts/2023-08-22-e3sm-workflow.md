---
layout: post
title: Updated E3SM Workflow
date: 2023-08-22 1:19
author: Tian
comments: true
categories: [E3SM]
tags: Null
---
I was recently requested to conduct a coupled E3SM simulation experiment. This task provids me with the opportunity to familiarize myself with the latest best-practice simulation standards. An official step-by-step guide can be found [here](https://acme-climate.atlassian.net/wiki/spaces/DOC/pages/2309226536/Running+E3SM+step-by-step+guide).

## Some Key Takeaways:
- **Comprehensive Scripting:** Maintain all aspects within the script, including cloning the source code, building the project, and altering the namelist. This practice helps prevent future mistakes.
- **Script Version Control:** Ensure that each simulation is associated with a specific script version, enabling better tracking and replication of results.
- **Test the Simulation before Production Runs:** Conduct small PE-layout test runs over a few days to verify the stability of the simulation. This preliminary testing ensures that the simulation does not crash and can produce bit-for-bit (b4b) results in comparison to the baseline runs or across various PE layouts. Such testing minimizes the risk of a production run crashing after an extended wait time in the queue.
- **Post-Processing Version Control:** Utilize version control for the post-processing control file, for tools such as [zppy](https://e3sm.org/resources/tools/end-to-end-processing/zppy/), to maintain consistency across various stages of analysis.
- **Archiving Simulation Results:** Always archive the simulation results, making it easier to locate past results and restart the simulations if necessary.

## Steps:
- **Start from an exsisting simulation script:** Use a script like [this one](https://github.com/E3SM-Project/SimulationScripts/blob/master/archive/CoupledGroup/v3_dev/run.20230808.v3alpha02.piControl.chrysalis.sh).
- **Reproducing Results:**
    - To reproduce the same results, submit the job with a short test option activated.
    - Ensure `do_fetch_code=true` to enable the script to automatically clone the code and check the correct branch.
- **Short Test and Verification:**
    - After the short test, varify the test results with the original simulation results by using the `md5sum` function:
    ```fortran
        # 10-day test simulations with different layouts
        cd /lcrc/group/e3sm/ac.tian.zhou/E3SMv3_dev/20230808.v3alpha02.piControl.chrysalis/tests
        for test in *_*_ndays
        do
            zgrep -h '^ nstep, te ' ${test}/run/atm.log.*.gz | sort -n -k 3,3 | uniq > atm_${test}.txt
        done
        # Reference simulation from original runs (log files extracted using zstash)
        zgrep -h '^ nstep, te ' /lcrc/group/e3sm/ac.golaz/E3SMv3_dev/20230808.v3alpha02.piControl.chrysalis/original/archive/logs/atm.log.347003.230622-141836.gz | sort -n -k 3,3 | uniq | head -n 482 > atm_ref.txt
        # Verification
        md5sum *.txt
        ac18d8e307d5f474659dffbde3ddf0d3  atm_ref.txt
        ac18d8e307d5f474659dffbde3ddf0d3  atm_XS_1x10_ndays.txt
    ```
- **Modifications and Further Testing:**
    - Modify the simulation case as needed.
    - If the source code remains the same, no need to rebuild the model for other tests or production runs. Set `do_fetch_code=false` and `do_case_build=false`; others remain `true`.
- **Production Run and Archiving:**
    - Once the production run is done, consider archiving the results.
    - First compress log files from failed runs. Make sure not to compress the log files from an active simulation, this will cause the model to crash
    ```bash
        cd /lcrc/group/e3sm/ac.tian.zhou/E3SMv3_dev/20230808.v3alpha02.piControl.chrysalis/run
        gzip *.log.372164.230811-102057 
    ```
    - Repeat for all the log file time stamps, except the current one.
    - Short term archiving for the first 50 years (siumulation was from 0001 to 0050)
    ```bash
        cd /lcrc/group/e3sm/ac.tian.zhou/E3SMv3_dev/20230808.v3alpha02.piControl.chrysalis/case_scripts
        ./case.st_archive --last-date 0051-01-01 --force-move --no-incomplete-logs
    ```
- **Postprocessing using zppy:**
    - Prepare zppy configuration file. You can start with an exsisting one
    ```bash
        cd ~/E3SMv3_dev/scripts
        cp /home/ac.golaz/E3SMv3_dev/scripts/post.20230802.v3alpha02.1pctCO2_0101.chrysalis.cfg .
    ```
    - Rename and customize the file content.
    - Run zppy
    ```bash
        # first load e3sm_unified 
        # on Chrysalis
        source /lcrc/soft/climate/e3sm-unified/load_latest_e3sm_unified_chrysalis.sh
        # on Compy
        source /share/apps/E3SM/conda_envs/load_latest_e3sm_unified_compy.sh
        # on NERSC Perlmutter
        source /global/common/software/e3sm/anaconda_envs/load_latest_e3sm_unified_pm-cpu.sh   
        # then run zppy
        zppy -c <your_file>.cfg
    ```
    - If everything runs okay, the analysis results will be availble to the public like [this](https://web.lcrc.anl.gov/public/e3sm/diagnostic_output/ac.tian.zhou/E3SMv3_dev/longpipe/20230808.v3alpha02.piControl.chrysalis/)
    - If you want to compare the zppy-generated plots using the Interface for InterComparision of E3SM (IICE) between simulations, you could use these two links for [E3SM_Diags](https://portal.nersc.gov/project/e3sm/iice/) and for [MPAS](https://portal.nersc.gov/project/e3sm/iice/mpas-a/). 
- **Documenting the simulation:**
  - Use this [template](https://acme-climate.atlassian.net/wiki/spaces/EWCG/pages/2297299190/Simulation+Run+Template) to document the simulation.
  - [PACE](https://pace.ornl.gov/) provides detailed performance information for simulations. Provide job ID or search username to find the simulation.

## Additional Notes:
- Quota check for Compy and Chrysalis
    - Compy: `lfs quota -hu zhou014 /compyfs`
    - Chrysalis: `/usr/lpp/mmfs/bin/mmlsquota -u ac.tian.zhou --block-size T fs2`
