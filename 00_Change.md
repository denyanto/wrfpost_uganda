To change your Conda repository configuration from Anaconda defaults to conda-forge, run the following commands in your terminal or Anaconda Prompt::

```console
user=ict-10
cp -r /scratch/den/conda_env/wrfpost /home/${user}/.conda/envs
ml restore intel
conda activate wrfpost
```

Create working folder
```console
mkdir wrfpost
cd wrfpost
```

Identify your wrf output
```console
ln -sf /scratch/wido/uganda/wrfout_d01_2026-04-01_00:00:00 .
```
