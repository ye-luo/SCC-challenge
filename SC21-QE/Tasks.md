# Introduction
Quantum ESPRESSO is an integrated suite of Open Source computer codes for electronic structure calculations and materials modeling at the nanoscale. It is based on density functional theory, plane waves, and pseudopotentials.
For more information about the code see: https://www.quantum-espresso.org/project/manifesto

# Instructions and Necessary Components
Compile QE from the develop branch from Gitlab https://gitlab.com/QEF/q-e.
Verified reference commit is e17b9b110d8fb1a2ece30dbadb4ef4a01de350a0.

Instructions for building are https://gitlab.com/QEF/q-e/-/wikis/Developers/CMake-build-system
You will need:
- required: a compiler (e.g. GNU, NvidiaHPC), an MPI interface (e.g. OpenMPI), CMake
- optional: CUDA, parallel eigensolvers, vendor-optimized BLAS, LAPACK FFT math libraries.

For this competition, you will be asked to validate your build (task0) with the standard test suite (always good practice on a new system), and then run three different benchmarking inputs.

The needed input files can be found under the task{1,2,3}-XXX folders.
The output log will be printed to stdout (submission part A), and you'll need to submit these for scoring.
Please name the file as `teamX_QE_taskX_printout.txt`

In addition, you'll also need to submit
- What is the cloud resource configuration for this specific run (submission part B) including Instance node types, the number of nodes and the computed cost for this run.
- Your job scripts which include mpirun command-line and all the environment variables if there are (submission part C).
Please include B and C in one file and name it as `teamX_QE_taskX_resource_and_runscript.txt`.

For scoring, we measure three things: correctness, time to solution, and cost in terms of computation resources used. 
- Correctness: We'll compare your resulting energy to the reference one to see if it's correct.
- Time to solution: This is the **wall** time (not CPU time) printed out at the end of the output. `PWSCF        :  22m25.98s CPU  13m11.91s WALL`
- Cost: This measures how well you used the Azure budget on the computation. It is the (Time to solution) * (Cost on Azure to run the whole computation). For example, if it cost $2/hour to run on a Skylake, and it took 4 hours to run the benchmark, then the cost is ($2/hour)*(4 hours) = $8. 

## Do's and Don'ts
You may
- Use mpirun options to control the number of MPI ranks, resource mapping and affinity.
- Use OpenMP environment variables to control OpenMP behavior.
- Use pw.x command line options to finely tune the parallelization schemes used in the runs.
  Options are documented in QE User's Guide. Section 3.3 Parallelism.

You should never
- Modify QE input files provided for the competition unless being informed.
- Modify QE output files.

**Any team that cheats by editing input/output files will be disqualified**


# Tasks and Scoring

## Task 0: Running the standard tests
Run the standard tests: `ctest -L "system--pw" --output-on-failure`

Save the output and submit it (part A).

### Scoring: 
- 20 points for all tests passing. However, you need to run the tests on the systems which you report results on for the benchmarks (Tasks 1, 2, 3) . This is to validate that the code executes correctly on that system So if you report results Task 1 on a Skylake, you need to run the standard tests on a Skylake to get these points. So 20/3 points for running the tests on the system you used for Tasks 1, 2, and 3. 

## Task 1: Benchmark run with the ausurf input
Run the ausurf input: `ausurf.in`
Save the output and submit part A,B,C

### Scoring:

- 10 points for correct converged resulting energy
- 10 points for lowest cost run, 9 points for second lowest, 8 points for third lowest, etc., from ranking cost across teams

## Task 2: Benchmark run with psiwat input
Run the psiwat input: `psiwat.in`
Save the output and submit part A,B,C

### Scoring:

- 10 points for correct converged resulting energy
- 10 points for lowest cost run, 9 points for second lowest, 8 points for third lowest, etc., from ranking cost across teams
- 10 points for lowest (fastest) time to solution, 9 points for second lowest, 8 points for third lowest, etc., from ranking time across teams


## Task 3: Benchmark run with TiO2 Brookite input
Run the TiO2 Brookite input : `TiO2-brookite-scf.in`
Save the output and submit part A,B,C

### Scoring:

- 10 points for correct converged resulting energy
- 10 points for lowest cost run, 9 points for second lowest, 8 points for third lowest, etc., from ranking cost across teams
- 10 points for lowest (fastest) time to solution, 9 points for second lowest, 8 points for third lowest, etc., from ranking time across teams

# Debugging and Troubleshooting
- If you want to be sure you're running on a GPU, you can use nvidia-smi to try to understand where your code is running
- Keep in mind that the code has memory requirements, so you might need to use more resources to meet the memory needs of specific runs.
- https://www.quantum-espresso.org/Doc/user_guide.pdf

# Scoring sheet

| Task                    | Score |
| ----------------------- | ----- |
| Task 0                  | 20    |
| Task 1 correctness      | 10    |
| Task 1 cost             | 10    |
| Task 2 correctness      | 10    |
| Task 2 cost             | 10    |
| Task 2 time-to-solution | 10    |
| Task 3 correctness      | 10    |
| Task 3 cost             | 10    |
| Task 3 time-to-solution | 10    |
| Total                   | 100   |



