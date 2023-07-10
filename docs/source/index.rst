Welcome to the Jupyter notebook on the UMCG Genetics HPC cluster instructions
===================================

This assumes you have set up cluster access, for Windows users this assumes you have git for windows set up and are running the terminal commands there: https://ssh-and-rsync-for-windows.readthedocs.io/en/latest/

log into the cluster like you normally would (ssh airlock+gearshift in the terminal)

.. code-block:: console

   load conda
   ml Anaconda3

create a conda environment which has jupyter-lab in it, or add jupyter-lab to an existing conda environment
for a new environment for example:

.. code-block:: console

   conda create -n jupyter_env -c conda-forge jupyterlab

if you do not have a screen session and interactive session to run your python code in yet, create them:

.. code-block:: console

   screen -S jupyter # this opens a screen session

.. code-block:: console

   srun --cpus-per-task=4 --mem=64gb --nodes=1 --qos=priority --job-name=jupyter --time=23:59:59 --tmp=1000gb --pty bash -i # this requests an interactive session

you might not immediatly get your interactive session. you can leave the screen and thus interactive session using CTRL+A, then D. This leaves the session without closing it. you can later go back to it using screen -r jupypter, and leave it again with CTRL+A, D.

once you have resources, you can open the conda environment

.. code-block:: console

   source activate jupyter_env # or however you named the environment

next we are going to run jupyter notebook on a specific port, choose a port, and be sure to check if it is not already in use by using lsof -i:8824 in the terminal. If there is no output, the port is free, if there is output, you need to select a different port


start the jupyter notebook server with

.. code-block:: console

   jupyter-lab --no-browser --port=8824 # or some other port

make sure you are not using a port someone else is already using

this will start the server, and it will show some output. you want the text that shows you the token: /lab/?token=lettersandnumbers
you want to note that part of the text

you can leave this screen session again with CTRL+A, D, and leave it running in the background.

additionally, you want to see what node you are connected to with your interactive job. You can use squeue -u ${USER} to show what jobs you are running and on which node this is. You should be able to see your interactive job and the node it is on

next you want to go to a terminal (window) on your own machine that is not connected to the cluster, and run a command to connect to that specific node and port

.. code-block:: console

   ssh -N -f -L localhost:8887:localhost:8855 umcg-username@airlock+gs-vcompute09 # where the second number is the port you supplied earlier, the username is your username, and the gs-vcompute is the node you are running the interactive job on

now, you want to open your local browser at localhost:8887/lab/?token=thetokenyounoted, using of course that token you noted. This should open the jupyter notebook environment on the cluster, right in your browser.


.. note::

   This project is under active development.

Contents
--------

.. toctree::

   
