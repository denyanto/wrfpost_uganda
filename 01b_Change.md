First update your .bashrc to allow your winscp application.
```console
vi /home/${user}/.bashrc
```
Add the new line with:
```console
source /software/compilers/intel-oneapi/2025.2.1/setvars.sh > /dev/null 2>&1
```

The next step is download the winscp from https://winscp.net/download/WinSCP-6.5.6-Setup.exe/download 

After download, then install it! After that klik the application!







<img width="349" height="379" alt="image" src="https://github.com/user-attachments/assets/a43a5e93-1602-42e5-b236-f942b42e4e98" />

You have accesses your hpc.




<img width="482" height="321" alt="image" src="https://github.com/user-attachments/assets/fe74544a-d77c-4265-9708-184ef1ecf176" />

To change your Conda configuration, run the following commands in your terminal or Anaconda Prompt::

```console
user=ict-10
cp -r /scratch/den/conda_env/wrfpost /home/${user}/.conda/envs
cp -r /scratch/den/conda_env/cartopy /home/${user}/.local/share
source /software/compilers/intel-oneapi/2025.2.1/setvars.sh
conda activate wrfpost
```

Create working folder
```console
mkdir wrfpost
cd wrfpost
```

Identify your wrf output
```console
mkdir wrfoutput
ln -sf /scratch/wido/uganda/wrfout_d01_2026-04-01_00:00:00 wrfoutput/
```
