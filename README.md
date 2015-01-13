# cisst-saw-doxygen

Repository used to create and host the cisst/SAW reference manual automatically generated using Doxygen (http://www.stack.nl/~dimitri/doxygen/manual).

The generated reference manual can be found at: http://jhu-cisst.github.io/cisst-saw-doxygen

# Instructions

To generate the reference manual:
- clone (`--recursive -b master`) cisst-saw from http://github.com/jhu-cisst/cisst-saw
- in the cloned directory `cisst-saw`, clone this repository: `git clone http://github.com/jhu-cisst/cisst-saw-doxygen`
- in the `cisst-saw-doxygen` directory, checkout the `gh-pages` branch: `git checkout gh-pages`
- run Doxygen: `doxygen Doxyfile`
- make sure new files generated are checked in: `git add .`
- commit and push: `git commit -a -m "updated generated reference manual"; git push origin gh-pages`

