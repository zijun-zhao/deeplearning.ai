#### This file will record all problems I have encountered during programming

##### 8 Aug 2019

1. package tidyverse in **R**

```R
there is no package called ‘tidyverse’
```

2. Remove the last n characters from a list in **R**

```R
gsub('.{n}$', '', list_tobechanged)
```

3. range(1,n) in **R**
```R
seq(1,n-1)
```

4. ERROR *\`gap.degree\` parameter has length larger than 1* in **R**

First print out circos.par["gap.degree"] to check where is wrong
```R
circos.par["gap.degree"]
```
If the current_value is not equal to the default value, just set:
```R
circos.par(gap.degree = 1)
```

5. Error *plot.new() : figure margins too large* in **R**
```R
par(mar=c(1,1,1,1))
```

6. *Error in read.table(file = file, header = header, sep = sep, quote = quote, : first five rows are empty: giving up* in **R** and **PyThon**:

When output the dataframe value in a dictionary, only when print the dataframe will output nonempty .csv file. I do not know the reason.
```PyThon
for iter_key in dicHIER.keys():
    tempDF = dicHIER[iter_key]
    print(tempDF)
    (tempDF).to_csv('/home/penglab/Documents/dataSource/dfSET/HIER/'+str(iter_key)+'.csv')
```

 Also, no need to specify the tempDF as pd.DataFrame object, otherwise the output will still be empty.
 ```PyThon
    #WRong example
    pd.DataFrame(tempDF).to_csv('/home/penglab/Documents/dataSource/dfSET/HIER/'+str(iter_key)+'.csv')
```

7. *Error in col2rgb(col, alpha = TRUE) : invalid RGB specification* in **R**

The way to initialize a character object in R is to use c()
```R
col = c("#FFFFB3" , "#FF7F00", "#FF7F00" )
```

##### 5 Sep 2019
1. ERROR *there is no package called ‘devtools’* in **R**

In the shell:
```Shell
sudo yum -y install libcurl libcurl-devel
```

2.  ERROR *jupyter-client has to be installed but “jupyter kernelspec --version” exited with code 127* in **R**

In the shell:
```Shell
sudo yum update -y
sudo nano /etc/yum.repos.d/jags.repo
```

With the following content in it:
```Shell
[home_cornell_vrdc]
name=Sundry packages for scientific computing (CentOS_7)
type=rpm-md
baseurl=http://download.opensuse.org/repositories/home:/cornell_vrdc/CentOS_7/
gpgcheck=1
gpgkey=http://download.opensuse.org/repositories/home:/cornell_vrdc/CentOS_7//repodata/repomd.xml.key
enabled=1
```

Then:
```Shell

sudo yum groups mark convert

sudo yum install epel-release -y

sudo yum groupinstall "Development tools" -y

sudo yum install wget openblas java-1.8.0-openjdk-devel zlib-devel libicu-devel libpng-devel libcurl-devel libxml2-devel openssl-devel openmpi-devel numpy python-matplotlib netcdf4-python netcdf-devel netcdf python-pandas python-basemap proj-epsg proj-devel gdal-devel monitorix gnuplot ImageMagick librsvg2-devel libsodium-devel libwebp-devel cairo-devel hunspell-devel openssl-devel poppler-cpp-devel protobuf-devel mariadb-devel mysql-devel v8-devel redland-devel cyrus-sasl-devel libtiff-devel tcl-devel tk-devel xauth mesa-libGLU-devel glpk-devel libXt-devel gsl-devel fftw-devel bzip2-devel geos-devel gtk2-devel gtk3-devel libjpeg-turbo-devel jags4-devel bwidget blas-devel lapack-devel mpfr-devel unixODBC-devel libsndfile-devel udunits2-devel postgresql-devel libRmath-devel qt-devel libdb-devel octave-devel hiredis-devel poppler-glib-devel QuantLib-devel boost-devel czmq-devel ImageMagick-c++-devel file-devel opencl-headers -y
```

Install CRAN R.
```Shell
sudo yum install R -y
```

Then replace the included libRblas.so with a better one from OpenBLAS.
```Shell
sudo mv /usr/lib64/R/lib/libRblas.so /usr/lib64/R/lib/libRblas.so_

sudo ln -s /usr/lib64/libopenblas.so.0 /usr/lib64/R/lib/libRblas.so
```

Set up R Java
```Shell
sudo env PATH=/usr/local/bin:$PATH R CMD javareconf
```

Set up OpenMPI.
```Shell
sudo sh -c "echo '/usr/local/lib' >> /etc/ld.so.conf"
sudo ldconfig
```

Install ggobi.
```Shell
wget http://www.ggobi.org/downloads/ggobi-2.1.11.tar.bz2

bunzip2 ggobi-2.1.11.tar.bz2

tar -xf ggobi-2.1.11.tar

cd ggobi-2.1.11

./configure --with-all-plugins

make

sudo make install

make ggobirc

sudo mkdir -p /etc/xdg/ggobi

sudo cp ggobirc /etc/xdg/ggobi/ggobirc

sudo ln -s /usr/local/lib/libggobi.so.0 /usr/lib64/libggobi.so.0

cd ..
```

Install Intel OpenCL.
```Shell
wget http://registrationcenter-download.intel.com/akdlm/irc_nas/9019/opencl_runtime_16.1.1_x64_rh_6.4.0.25.tgz

tar -xzf opencl_runtime_16.1.1_x64_rh_6.4.0.25.tgz

sudo yum install opencl_runtime_16.1.1_x64_rh_6.4.0.25/rpm/*
```

Install SYMPHONY.
```Shell
wget http://www.coin-or.org/download/source/SYMPHONY/SYMPHONY-5.6.14.tgz

tar -xzf SYMPHONY-5.6.14.tgz

cd SYMPHONY-5.6.14

./configure

make

sudo make install

sudo cp -R include/* /usr/include/

sudo cp -R bin/* /usr/bin/

sudo cp -R lib/* /usr/lib64/

sudo cp -R share/* /usr/share/

cd ..
```

Install OpenBugs
```Shell
sudo yum install http://pj.freefaculty.org/EL/7/x86_64/openbugs-3.2.3-1.x86_64.rpm
```

Install CUDA
```Shell
sudo yum-config-manager --add-repo http://developer.download.nvidia.com/compute/cuda/repos/rhel7/x86_64/cuda-rhel7.repo
sudo yum clean all
sudo yum -y install nvidia-driver-latest-dkms cuda
```

Creat File
```Shell
sudo nano /etc/profile.d/custom.sh
```
with content
```
export CPATH=\$CPATH:/usr/include/openmpi-x86_64
PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}
```

Download the actuar package source from the CRAN website and extract it.
```Shell
wget https://cran.r-project.org/src/contrib/actuar_1.2-2.tar.gz

tar -xzf actuar_1.2-2.tar.gz
```

