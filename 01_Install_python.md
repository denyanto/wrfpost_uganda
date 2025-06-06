# Install Python
1. Download miniconda
   ```console
   >> wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
   ```   
3. Install miniconda first
   ```console
   >> chmod a+x Miniconda3-latest-Linux-x86_64.sh
   >> ./Miniconda3-latest-Linux-x86_64.sh
   ```
   Follow the instructions!
   ```console
   Welcome to Miniconda3 py313_25.3.1-1
   
   In order to continue the installation process, please review the license
   agreement.
   Please, press ENTER to continue
   >>>
   ```
   Until you see this, type yes!
   
   ```console
   DISCLAIMER: THIS SOFTWARE IS PROVIDED BY ANACONDA "AS IS" AND ANY EXPRESS OR IMP
   LIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHA
   NTABILITY, FITNESS FOR A PARTICULAR PURPOSE , AND NON-INFRINGEMENT ARE DISCLAIME
   D. IN NO EVENT SHALL ANACONDA BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SP
   ECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCU
   REMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINE
   SS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTR
   ACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN AN
   Y WAY OUT OF THE USE OF MINICONDA(R), EVEN IF ADVISED OF THE POSSIBILITY OF SUCH
    DAMAGE.
   
   
   Do you accept the license terms? [yes|no]
   >>> yes
   ```
   Change location if you whish!
   ```console      
   Miniconda3 will now be installed into this location:
   /home/trainer/miniconda3   
     - Press ENTER to confirm the location
     - Press CTRL-C to abort the installation
     - Or specify a different location below   
   [/home/trainer/miniconda3] >>> /home/trainer/.miniconda3
   ```
   Follow the instructions! klik yes
   ```console
   conda config --set auto_activate_base false
   You can undo this by running `conda init --reverse $SHELL`? [yes|no]
   [no] >>> yes
   ```
   Close and re-open your current shell!
   ```console
   >> exit
   ```
5. Install python and the modules
   ```console
   >> conda create -n wrfpostpy python=3.12 netCDF4 cartopy scipy
   >> conda activate wrfpostpy
   ```
   Activating wrfpostpy!
   ```console   
   (base) [trainer@litbang-master ~]$ conda activate wrfpostpy
   (wrfpostpy) [trainer@litbang-master ~]$
   ```
   Installing wrf-python module!
   ```console   
   >> conda install conda-forge::wrf-python
   ```

6. Finish
