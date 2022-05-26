:author: Orhan Eroglu
:email: oero@ucar.edu
:institution: National Center for Atmospheric Research
:corresponding:

:author: Michaela Sizemore
:email: misi1684@ucar.edu
:institution: National Center for Atmospheric Research

:author: Anissa Zacharias
:email: anissaz@ucar.edu
:institution: National Center for Atmospheric Research

:author: Alea Kootz
:email: akootz@ucar.edu
:institution: National Center for Atmospheric Research

:author: Heather Craker
:email: hcraker@ucar.edu
:institution: National Center for Atmospheric Research

:author: John Clyne
:email: clyne@ucar.edu
:institution: National Center for Atmospheric Research


:bibliography: references

:video: https://www.youtube.com/watch?v=34zFGkDwJPc

---------------------------------------------------------------------------------------------------------------------------
The Geoscience Community Analysis Toolkit: An Open Development, Community Driven Toolkit in the Scientific Python Ecosystem
---------------------------------------------------------------------------------------------------------------------------

.. class:: abstract

The Geoscience Community Analysis Toolkit (GeoCAT) team develops and maintains
data analysis and visualization tools on structured and unstructured grids for
the geosciences community in the Scientific Python Ecosystem (SPE). In response to
dealing with increasing geoscientific data sizes, GeoCAT prioritizes scalability,
ensuring its implementations are scalable from personal laptops to HPC clusters.
Another major goal of the GeoCAT team is to ensure community involvement throughout
the whole project lifecycle, which is realized through an open development mindset
by encouraging users and contributors to get involved in decision-making.

.. class:: keywords

   data analysis, geocat, geoscience, open development, open source, scalability,
   visualization

Introduction
------------

The Geoscience Community Analysis Toolkit (GeoCAT) team, established in 2019,
leads the software engineering efforts of the National Center for Atmospheric
Research (NCAR)’s “Pivot to Python” initiative :cite:`pivot19`. The initiative
had an initial two-year roadmap with major milestones being: (1) Replicating
NCL’s computational routines in Python, (2) training and support for
transitioning NCL users into Python, and (3) moving tools into an open development
model. GeoCAT aims to create scalable data analysis and
visualization tools on structured and unstructured grids for the geosciences
community in the SPE. The GeoCAT team is committed to open development, which
helps the team prioritize community involvement at any level of the project
lifecycle alongside having the whole software stack open-sourced.

GeoCAT has created several, now-established, Python tools that are hosted and
managed publicly on GitHub to develop computation and visualization functions.
These tools are built on cornerstone Pangeo :cite:`pangeo18` (a community platform
for big data geoscience) packages such as Xarray :cite:`xarray17`, Dask
:cite:`dask15`, as well as Jupyter Notebooks. Dask
compatibility allows the GeoCAT-comp functions to scale from personal laptops to
high performance computing systems such as NCAR’s Casper, Cheyenne, and upcoming
Derecho clusters :cite:`ams22craker`.

Briefly, GeoCAT-comp houses computational operators for applications ranging from
regridding and interpolation, to climatology and meteorology. GeoCAT-examples
provides over 140 publication-quality plotting scripts in Python for Earth
sciences. It also houses Jupyter notebooks with high-performance, interactive
plots that enable features such as pan and zoom on fine-resolution geoscience
data (e.g. ~3 km data rendered within a few tens of seconds to a few minutes on
personal laptops). GeoCAT-viz enables higher-level implementation of Matplotlib
and Cartopy plotting capabilities through its variety of easy to use
visualization convenience functions. GeoCAT also maintains
WRF-Python (Weather Research and Forecasting), which works with WRF-ARW model
output and provides diagnostic and interpolation routines.

GeoCAT was recently awarded Project Raijin, which is an NSF EarthCube-funded
effort :cite:`raijinaward21`. Its goal is to enhance the open-source analysis
and visualization tool landscape by developing community-owned, sustainable,
scalable tools that facilitate operating on unstructured climate and global
weather data in the SPE. Throughout this three-year project, GeoCAT will work
on the development of data analysis and visualization functions that operate
directly on the native grid as well as establish an active community of
user-contributors.

This paper will provide insights about GeoCAT's software stack and
current status, team scope and near-term plans, open development methodology,
as well as current pathways of community involvement.

GeoCAT Software
---------------

The software tools maintained by GeoCAT implement
continuous integration and continuous delivery/deployment (CI/CD).

Continuous Integration and Continuous Delivery/Deployment (CI/CD)
=================================================================

GeoCAT employs a continuous delivery model, with 
a monthly package release cycle on package management systems and package indexes
such as Conda :cite:`conda` and PyPI :cite:`conda`. This model helps the team make
new functions available as soon as they are implemented and address potential errors
quickly. To assist this process, the team utilizes multiple tools
throughout GitHub assets to ensure automation, unit testing and code coverage, as
well as licensing and reproducibility. Figure :ref:`fig1badges`, for example,
illustrates the set of badges from one of our software tools, GeoCAT's computational
component, GeoCAT-comp, which shows the near real-time status of each CI/CD
implementation at any time in the GitHub repository homepage.

CI build tests of our repositories are implemented and
automated (for pushed commits, pull requests, and daily scheduled execution) via
GitHub Actions workflows :cite:`githubactions`, and the "CI" badge displays the status of those workflows.
Similarly, the "CONDA-BUILDS" badge shows if the
conda recipe works successfully for the repository. The percent code coverage through
unit tests are being analyzed with the "codecov" Python package :cite:`codecov`, and
its overall results as well as details per code script can be seen via the "COVERAGE"
badge. Each of our software repositories has a corresponding documentation page that
is populated mostly-automatically through Sphinx Python documentation generator
:cite:`sphinx` and published through ReadTheDocs :cite:`readthedocs` via an automated
building and versioning schema. The "DOCS" badge provides a link to the documentation page
as well as shows failures, if any, with the documentation rendering process. Figure
:ref:`fig2rtd` shows the documentation homepage of GeoCAT-comp for instance. The
"NCAR" and "PYPI" badges in the "Package" row shows and links to the latest versions
of the software tool distributed through NCAR's Conda channel and PyPI, respectively.
The "LICENSE" badge provides a link to our software licenses, which is Apache License
version 2.0 for all of the GeoCAT stack that enables the redistribution of the
open-source software products on an "as is" basis. Finally, to provide reproducibility
of our software products (either for the latest or any older version), we publish
version-specific Digital Object Identifiers (DOIs), which can be accessed through the
"DOI" badge. This allows the end-user to accurately cite the specific version of the
GeoCAT tools they used for reproducibility.

.. figure:: figures/fig1_badges.png
   :scale: 35%
   :figclass: bht

   GeoCAT-comp's badges in the beginning of its README file (i.e. the home page of
   the Githug repository) :cite:`geocatcomprepo` :label:`fig1badges`

.. figure:: figures/fig2_rtd.png
   :align: center
   :scale: 30%
   :figclass: w

   GeoCAT-comp documentation homepage built with Sphinx using a theme provided by
   ReadTheDocs :cite:`geocatcomprtd` :label:`fig2rtd`

GeoCAT-comp (and GeoCAT-f2py)
=============================

GeoCAT-comp is the computational component of the GeoCAT project as can be seen in
Figure :ref:`fig3structure`, and it houses implementations of geoscience data analysis
functions. Novel research and development is conducted for analyzing structured and
unstructured grid data from various research fields including climate,
weather, atmosphere, ocean, etc. In addition, some of the functionalities of
GeoCAT-comp are modernized versions of functions from NCAR Command Language (NCL).

Spherical harmonics (both decomposition and recomposition as well as area
weighting); Fourier transforms such as band-block, band-pass, low-pass, and
high-pass; meteorological variables such as relative humidity, dew point temperature,
heat index, saturation vapor pressure, etc.; climatology functions such as climate
average over multiple years, daily/monthly/seasonal averages, as well as anomalies;
regridding of curvilinear grid to rectilinear grid, unstructured grid to rectilinear
grid, curvilinear grid to unstructured grid, and vice versa; interpolation methods
such as bilinear interpolation of a rectilinear to another rectilinear grid,
hybrid-sigma levels to isobaric levels, and sigma to hybrid coordinates; empirical
orthogonal function (EOF) analysis; are some of the functions that are made
available through GeoCAT-comp. GeoCAT-comp documentation :cite:`geocatcomprtd`
provides signatures and descriptions of these functions as well as links to
usage examples.

.. figure:: figures/fig3_structure.png
   :align: center
   :scale: 50%
   :figclass: w

   GeoCAT project structure with all of the software tools
   :cite:`geocatcontribguide` :label:`fig3structure`

Many of the computational functions in GeoCAT are implemented in pure Python.
However, there are others that remain implemented in Fortran, but are wrapped
in Python. The whole GeoCAT-comp structure is split
into two repositories with respect to being either pure Python or Python with
compiled code (i.e. Fortran) implementations. Such implementation layers are
handled with the GeoCAT-comp and GeoCAT-f2py repositories respectively.

GeoCAT-comp's code-base does not explicitly contain or require any compiled
code, making it more accessible to the general Python community.
Additionally, GeoCAT-f2py is automatically installed through GeoCAT-comp's
installation, and all functions contained in the "geocat.f2py" package are
imported transparently into the "geocat.comp" namespace. Thus, GeoCAT-comp
serves as a user API to access the entire computational toolkit even though
its GitHub repository itself only contains pure Python code from the
developer’s perspective. Whenever prospective contributors want to contribute 
computational functionality in pure Python, GeoCAT-comp is the only
GitHub repository they need to deal with. Therefore, there is no onus on contributors
of pure Python code to build, compile, or test any compiled code
at GeoCAT-comp level.

GeoCAT-examples (and GeoCAT-viz)
================================

GeoCAT-examples :cite:`geocatexamplesrepo` was created to address a few of
the original milestones of NCAR's "Pivot to Python" initiative: (1) to
provide the geoscience community with well-documented visualization examples
for several plotting classes in the SPE, and (2) to help transition NCL users
into the Python ecosystem through providing such resources. It was born in
early 2020 as the result of a hackathon event among the GeoCAT team
and several other NCAR collaborators.
It has since grown to house novel visualization examples and showcase
the capabilities of other GeoCAT endeavors like GeoCAT-comp, along with
newer technologies like interactive plotting notebooks. Figure
:ref:`fig4cmap` illustrates one of the unique GeoCAT-examples cases that
was aimed at exploring the best practices for data visualization
like choosing color blind friendly colormaps.

.. figure:: figures/fig4_cmap.png
   :scale: 28%
   :figclass: bht

   Comparison between NCL (left) and Python (right) when choosing a
   colormap; GeoCAT-examples aiming at choosing color blind friendly
   colormaps :cite:`ams22sizemore` :label:`fig4cmap`

The GeoCAT-examples :cite:`geocatexamplesrtd` gallery contains over 140
example Python plotting scripts, demonstrating functionalities from Python
packages like Matplotlib, Cartopy, Numpy, and Xarray. The gallery includes
plots from a range of visualization categories such as box plots, contours,
meteograms, overlays, projections, shapefiles, streamlines, trajectories,
and more. In addition, it houses plotting examples for individual GeoCAT-comp
analysis functions.

Despite Matplotlib and Cartopy's capabilities to reproduce almost all
of NCL plots, there was one significant caveat with using their low-level
implementations against NCL: NCL's high-level plotting functions allowed
scientists to plot most of the cases in only a few tens of lines of codes
(LOC) while the Matplotlib and Cartopy stack required writing a few
hundreds of LOC. In order to build a higher-level implementation on top of
Matplotlib and Cartopy while recreating the NCL-style plots (from vital
plotting capabilities not widely available in Python ecosystem
at the time such as Taylor diagrams, and curly vectors, to more
stylistic changes that resemble NCL plots), the GeoCAT-viz library :cite:`geocatvizrepo` has been
developed. Use of functions from this library in GeoCAT-examples
significantly reduces the LOC requirements for most of the visualization
examples to comparable numbers to those of NCL's. Figure :ref:`fig5viz`
shows Taylor diagram and curly vector examples that have been created with
the help of GeoCAT-viz.

.. figure:: figures/fig5_viz.png
   :scale: 28%
   :figclass: bht

   Taylor diagram and curly vector examples that created with the help of GeoCAT-viz :label:`fig5viz`

WRF-Python
==========

WRF-Python was created in early 2017 in order to replicate NCL's Weather
Research and Forecasting (WRF) package in the SPE. About two years later,
NCAR's “Pivot to Python” initiative was announced, and the GeoCAT team
has taken over development and maintenance of WRF-Python.

The package focuses on creating a Python package that eliminates the need
to work across multiple software platforms when using WRF datasets. It
contains more than 30 computational (e.g. diagnostic calculations, several
interpolation routines) and visualization routines that aim at reducing
the amount of post-processing tools necessary to visualize WRF output
files.

Even though there is no continuous development in WRF-Python as it is in
the rest of the GeoCAT stack, the package is still maintained
with timely responses and bug-fix releases to the issues reported by the
user community.

Project Raijin
--------------

“Collaborative Research: EarthCube Capabilities: Raijin: Community Geoscience
Analysis Tools for Unstructured Mesh Data”, i.e. Project Raijin, of the
consortium between NCAR and Pennsylvania State University has been awarded by
NSF 21-515 EarthCube for an award period of 1 September, 2021 - 31 August,
2024 :cite:`raijinaward21`. Project Raijin aims at developing community-owned,
sustainable, scalable tools that facilitate operating on unstructured climate
and global weather data :cite:`raijin`. The GeoCAT team is in charge of the
software development of Project Raijin, which mainly consists of implementing
visualization and analysis functions in the SPE to be executed on native
grids. While doing so, GeoCAT is also responsible for establishing an open
development environment, clearly documenting the implementation work, and
aligning deployments with the project milestones as well as SPE
requirements and specifications.

GeoCAT has created the
Xarray-based Uxarray package :cite:`uxarrayrepo` to recognize unstructured
grid models through partnership with the geoscience community groups. UXarray
is built on top of the built-in Xarray Dataset functionalities while
recognizing several unstructured grid formats (UGRID, SCRIP, Exodus for now).
Since there are more unstructured mesh models in the community than UXarray
natively supports, its architecture will also support addition of new models.
Figure :ref:`fig6raijin` shows the regularly structured “latitude-longitude”
grids versus a few unstructured grid models.

.. figure:: figures/fig6_raijin.png
   :scale: 20%
   :figclass: bht

   Regular grid (left) vs MPAS-A & CAM-SE grids :label:`fig6raijin`

The UXarray project has implemented data input/output functions for UGRID,
SCRIP, and Exodus, as well as methods for surface area and integration
calculations so far. The team is currently conducting using GitHub discussions
to talk with with community members who are interested
in unstructured grids research and development in order to prioritize
data analysis operators that will be implemented throughout the project
lifecycle.

Open Development
----------------

To ensure community involvement at any level in the development lifecycle,
GeoCAT is committed to an open development model. With this model, we not only
have our project stack open-sourced but also ensure most of the project assets
that are directly related to the software development lifecycle are publicly
accessible. In order to implement this model, GeoCAT provides all of its software
tools as GitHub repositories with public GitHub project boards and roadmaps,
issue tracking and development reviewing, comprehensive documentation for
users and contributors such as a Contributor’s Guide :cite:`geocatcontribguide` and
toolkit-specific documentation, along with community announcements on the GeoCAT
blog. Furthermore, GeoCAT encourages community feedback and contribution at any
level. As a result of this, numerous
community requests and feedback have played significant role in forming and revising
GeoCAT's roadmap and project scopes. We are hoping to increase the community
involvement in our development lifecycle.

Community engagement
--------------------

To further promote engagement with the geoscience community, GeoCAT organizes
and attends various community events.

The GeoCAT team has also encouraged undergraduate and graduate student
engagement in the Python ecosystem through participation in NCAR's Summer
Internships in Parallel Computational Science (SIParCS). GeoCAT-examples and
GeoCAT-viz in particular have received significant contributions through
SIParCS interns in past years.

Project Pythia
==============

The GeoCAT effort is also a part of the NSF funded Project Pythia. Project
Pythia aims to provide a public, web-accessible training resource that could
help educate earth scientists to more effectively use the SPE and cloud
computing for dealing with big data in geosciences. GeoCAT helps the Pythia
development with content creation as well as infrastructure contributions.

GeoCAT has also contributed several Python tutorials to educational resources
created through Project Pythia.
