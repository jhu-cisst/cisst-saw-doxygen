# cisst-saw-doxygen

Repository used to create and host the cisst/SAW reference manual automatically generated using Doxygen (http://www.stack.nl/~dimitri/doxygen/manual).

The generated reference manual can be found at: http://jhu-cisst.github.io/cisst-saw-doxygen

# Instructions

To generate the reference manual:
- clone (`--recursive -b master`) cisst-saw from http://github.com/jhu-cisst/cisst-saw and build as many libraries and components as possible
- clone this repository: `git clone http://github.com/jhu-cisst/cisst-saw-doxygen` anywhere you want
- in the `cisst-saw-doxygen` directory, checkout the `gh-pages` branch: `git checkout gh-pages`
- run CMake in the source directory for `cisst-saw-doxygen`.  You can't do out-of-source build, this is to make sure the generated pages are in the same git repository.   CMake will generate all the include directories and configure the file `Doxyfile`
- run Doxygen: `doxygen Doxyfile`
- make sure new files generated are checked in: `git add .`
- commit and push: `git commit -a -m "updated generated reference manual"; git push origin gh-pages`

