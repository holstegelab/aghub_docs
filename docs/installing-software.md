# Installation of Software Packages

AGHub has no direct internet connection. The usual workflow is:

1. Prepare software on your local machine.
2. Transfer it through Research Drive.
3. Install/run it on AGHub.

If you have not set up transfer yet, first read [Use of Research Drive](research-drive.md).

## Recommended: install with conda-pack

Use this when you need extra Python/R tools beyond the default AGHub environment.

### On your local machine

1. Create and activate a Conda environment:

```bash
conda create -n myenv
conda activate myenv
```

2. Install the packages you need:

```bash
conda install numpy
```

3. Install conda-pack and create an archive:

```bash
conda install conda-pack
conda-pack -n myenv -o myenv.txz
```

4. Copy the archive into your Research Drive folder.

> **Note:** If Conda is not installed locally, use [Miniforge](https://github.com/conda-forge/miniforge#mambaforge).

### On AGHub

1. Create target directory and unpack:

```bash
mkdir -p ~/envs/myenv
tar -xvf ~/rd/myenv.txz -C ~/envs/myenv
```

2. Activate and finalize:

```bash
conda activate ~/envs/myenv
conda-unpack
```

## Alternative: run software with Singularity/Apptainer

AGHub provides a prebuilt image at `/project/aghub/Share/images/conda.sif`.

Open a shell in the container:

```bash
singularity shell /project/aghub/Share/images/conda.sif
```

Run a command directly:

```bash
singularity run /project/aghub/Share/images/conda.sif ipython
```

> **Important:** You cannot run SLURM submission commands (for example `sbatch`) from inside the container.

## Continue with SLURM

After software setup, submit jobs with the Spider guide:
[Spider SLURM Getting Started](https://spiderdocs.readthedocs.io/en/latest/Pages/getting_started.html)
