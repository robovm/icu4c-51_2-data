Minimal ICU data file for RoboVM
===============

This is a modified version of [ICU 51.2's data sources (icu4c-51_2-data)](http://site.icu-project.org/download/51#TOC-ICU4C-Download) which builds a minimal `icudt51l.dat` file for use as default ICU data in RoboVM. With this data file `en` is the only available locale and any attempt to format dates and numbers using some other locale will fallback on the `en` locale's formatting rules. No break iterators and no collations are included.

## Build instructions

```bash
curl -O http://download.icu-project.org/files/icu4c/51.2/icu4c-51_2-src.tgz
tar xvfz icu4c-51_2-src.tgz
cd icu/source
rm -rf data
git clone git://github.com/robovm/icu4c-51_2-data.git data
./runConfigureICU MacOSX --with-data-packaging=archive
make
make
```
Note: For me the first `make` fails with an error. Rerunning `make` works.

The `icudt51l.dat` will be saved to `out/icudt51l.dat`

## Generate the header file for embedding in RoboVM

RoboVM embeds a gzipped version of the data file as code. Here's how to generate the header file for inclusion in RoboVM:

```bash
gzip --best -c out/icudt51l.dat > out/icudt51l.dat.gz
xxd -i out/icudt51l.dat.gz > out/icudt51l.dat.gz.h
LEN=`stat -f '%z' out/icudt51l.dat`; echo "unsigned int out_icudt51l_dat_len = $LEN;" >> out/icudt51l.dat.gz.h
```
