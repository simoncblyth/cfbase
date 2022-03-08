cfbase repo : Sharing Opticks CSGFoundry geometry
=====================================================


Tarball Layout
----------------

::

    epsilon:cfbase blyth$ tar ztvf tds3_mar_2022.tar.gz
    drwxrwxr-x  0 blyth  blyth       0 Mar  8 11:49 tds3_mar_2022/
    drwxr-xr-x  0 blyth  blyth       0 Mar  7 22:26 tds3_mar_2022/CSGFoundry/
    -rw-rw-r--  0 blyth  blyth    3680 Mar  8 11:18 tds3_mar_2022/CSGFoundry/meshname.txt
    -rw-rw-r--  0 blyth  blyth     190 Mar  8 11:18 tds3_mar_2022/CSGFoundry/mmlabel.txt
    -rw-rw-r--  0 blyth  blyth     608 Mar  8 11:18 tds3_mar_2022/CSGFoundry/solid.npy
    -rw-rw-r--  0 blyth  blyth  207872 Mar  8 11:18 tds3_mar_2022/CSGFoundry/prim.npy
    -rw-rw-r--  0 blyth  blyth 1504128 Mar  8 11:18 tds3_mar_2022/CSGFoundry/node.npy
    -rw-rw-r--  0 blyth  blyth  521920 Mar  8 11:18 tds3_mar_2022/CSGFoundry/tran.npy
    -rw-rw-r--  0 blyth  blyth  521920 Mar  8 11:18 tds3_mar_2022/CSGFoundry/itra.npy
    -rw-rw-r--  0 blyth  blyth 3102656 Mar  8 11:18 tds3_mar_2022/CSGFoundry/inst.npy
    -rw-rw-r--  0 blyth  blyth 8766864 Mar  8 11:18 tds3_mar_2022/CSGFoundry/bnd.npy
    -rw-rw-r--  0 blyth  blyth    1695 Mar  8 11:18 tds3_mar_2022/CSGFoundry/bnd_meta.txt
    -rw-rw-r--  0 blyth  blyth   98432 Mar  8 11:18 tds3_mar_2022/CSGFoundry/icdf.npy
    -rw-rw-r--  0 blyth  blyth       3 Mar  8 11:18 tds3_mar_2022/CSGFoundry/icdf_meta.txt
    -rw-rw-r--  0 blyth  blyth      10 Mar  8 11:18 tds3_mar_2022/CSGFoundry/meta.txt
    epsilon:cfbase blyth$ 



How to render with Opticks/NVIDIA OptiX
------------------------------------------


1. create a directory to hold geometry, eg /tmp
2. unpack the tarball into the directory::
3. set CFBASE envvar and run render::

    export CFBASE=/tmp/tds3_mar_2022
    cx  ## cd ~/opticks/CSGOptiX
    source ./cxr.sh 

OR::

   CFBASE=/tmp/tds3_mar_2022 source ./cxr.sh 


Creation of tarballs with geocache-create-cfbase-tarball
----------------------------------------------------------

The tarball name and top level directory comes from OPTICKS_KEYFUNC envvar. 

::

    geocache-create-cfbase-tarball(){
       local msg="=== $FUNCNAME :"
       local keydir=$(geocache-keydir)
       local keyfunc=$(geocache-keyfunc)

       cd $keydir
       [ ! -d "CSG_GGeo" ] && echo $msg ERROR CSG_GGeo does not exist && return 1

       cd $keydir/CSG_GGeo
       [ ! -d "CSGFoundry" ] && echo $msg ERROR CSG_GGeo/CSGFoundry does not exist && return 2

       mkdir -p $keyfunc

       cd $keydir/CSG_GGeo/$keyfunc
       ln -svf ../CSGFoundry

       cd $keydir/CSG_GGeo

       local tgz=$keyfunc.tar.gz
       echo $msg creating tgz $tgz into directory $PWD containing the CSGFoundry directory  
       local ans 

       read -p "$msg proceed to create tgz $tgz into $PWD ? Enter YES to continue : " ans

       if [ "$ans" == "YES" ]; then

           case $(uname) in
               Darwin) tar -L -czvf $tgz $keyfunc ;;
               Linux)  tar -h -czvf $tgz $keyfunc ;;
           esac
       else
           echo $msg SKIPPED
       fi

       return 0
    }




