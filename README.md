2D-Debris
=========

Debris flow simulation code. Current status: ...in progress...

Shallow water equations are taken to solve numerically the dynamic of debris flor and avalanches. The numerical method is upwind fist order.
 

Steps to compile and run 2D_Debris and visualize outputs.
----------------------------------------------------------

0.- Mesh and init files format (ASCII):

    mesh Example: malla0.txt
     	  nvert		4
	  ncols         340
	  nrows         336
	  xllcorner     -582.5
	  yllcorner     -1189.0968913398
	  cellsize      5.0
	  NODATA_value  -9999
	  Z(1,1:ncols)
	  ...
	  Z(nrows,1:ncols)


    init Example: init0.txt
     	  nvert		4
	  ncols         340
	  nrows         336
	  xllcorner     -582.5
	  yllcorner     -1189.0968913398
	  cellsize      5.0
	  NODATA_value  -9999
	  h(1,1:ncols)
	  ...
	  h(nrows,1:ncols)

    where Z is the bed and h the debris depth.
    some users may ask me for a real mesh and initial conditions files, as well as a final - expected positions file.


1.- (Recommended for large mesh files) Create a binary mesh file. 

    1.1.- Check that you have the necessary files: 

    	  	preproc_mesh.f 

    	     	malla0.txt

    1.2.- Compile preproc_mesh.f and run it (This may take a few minutes):

    	  	gfortran preproc_mesh.f -o preproc_mesh

		./preproc_mesh

    1.3.- Check that output files have been created:

    	  	proc_mesh.dat (binary)

		preproc

    1.4.- If proc_mesh.dat exists (and preproc exists and contains just a '1') the mesh-read process will run much faster. You just need to do this the first time you run the code with this mesh file (malla0.txt) in this computer. If preproc contains a '0', then 2D_Debris needs to read and process the mesh. It runs but you will waste time.

2.- (Optional) Binary file for error estimation. You need a final/expected data file.

    2.1.- Check that you have the necessary files:

    	  	preproc_comparador.f

		malla0.txt

		h_obsfinalrot.tec

    2.2.- Compile preproc_comparador.f and run it (This may take a few minutes):

    	  	gfortran preproc_comparador.f -o preproc_comparador

		./preproc_comparador

    2.3.- If you want the simulation to process the expected position itself, just open the file 'err_proc' and type 0

3.- (Optional) Custom your program file. 

    3.1.- Run python script newson.py

    	      python newson.py

    3.2.- Answer questions.

4.- Just before running: 

    4.1.- Check that you have the necessary files

    	  	main2D.f 

	 	calc_paredes2D_Sf_fix3.f 

	 	matprod.f 

		read_regmesh2DP.f 

		compute_Sf2D.f 

		method2Drd.f 

		out_vtk.f 

		rutina_comparadorP.f

		err_proc

		preproc	

		malla0.txt

		params.txt

		time.data

    4.2.- Modify time.data (if needed)

    	  	 1st line: total simulated time

		 2nd line: time lag between vtk outputs

		 3rd line: time steps between standard outputs

    4.3.- Modify params.txt (if needed)

    	  	 Friction is modelled as: Sf = tan(phi) + C*V^2/h

		 where V is fluid velocity and h is fluid depth.

		 k is the 'earth pressure' Better if you do not touch it. 

    	  	 1st line: tan(phi) 

		 2nd line: C

		 3rd line: k

5.- Compile and run:

    gfortran main2D.f calc_paredes2D_Sf_fix3.f matprod.f read_regmesh2DP.f compute_Sf2D.f method2Drd.f out_vtk.f rutina_comparadorP.f -O3 -Wall -o main2D

    ./main2D

6.- Visualize results:

    If you created plot_****.vtk files, you may watch them in Paraview Viewer.