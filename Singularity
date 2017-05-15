BootStrap: docker
From: continuumio/miniconda:4.3.11


%setup
    echo "Looking in directory '$SINGULARITY_ROOTFS' for /bin/sh"
    if [ ! -x "$SINGULARITY_ROOTFS/bin/sh" ]; then
        echo "Hrmm, this container does not have /bin/sh installed..."
        exit 1
    fi
    exit 0


%post
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# this will install all necessary packages and prepare the container

    apt-get -y update
    apt-get -y install make gcc zlib1g-dev libncurses5-dev

    wget https://github.com/samtools/samtools/releases/download/1.3.1/samtools-1.3.1.tar.bz2 \
        && tar -xjf samtools-1.3.1.tar.bz2 \
        && cd samtools-1.3.1 \
        && make \
        && make prefix=/usr/local install

    export PATH=/opt/conda/bin:$PATH

    conda install --yes -c bioconda \
        star=2.5.2b \
        sailfish=0.10.1 \
        fastqc=0.11.5 \
        kallisto=0.43.0 \
        subread=1.5.0.post3 \
        bcftools=1.4.1 \
        bd2k-python-lib=1.14a1.dev37 \
        bx-python=0.7.3 \
        cutadapt=1.13 \
        gffutils=0.8.7.1 \
        htseq=0.7.2 \
        bedtools= 2.26.0 \
        pybedtools=0.7.9 \
        pyfaidx=0.4.8.1 \
        pysam=0.11.1 \
        emboss=6.5.7      
    conda install --yes -c r \
        r-base=3.2.1 
    conda clean --index-cache --tarballs --packages --yes


    mkdir -p \
        /oasis/tscc/scratch \
        /projects




%runscript


    echo "Arguments received: $*"
    #exec /usr/bin/python "$@"


    #!/bin/bash
    # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # this text will get copied to /singularity and will run whenever the container
    # is called as an executable


    function usage() {
        cat <<EOF

    NAME
    eclip - eclip RNA Binding Proteins peek calling pipeline 0.1.5

    SYNOPSIS
    eclip tool [tool options]
    eclip list
    eclip help

    DESCRIPTION
    Singularity container with tools to build eclip rnaseq pipeline.

    EOF
    }


    function tools() {
        echo "conda: $(which conda)"
        echo "---------------------------------------------------------------"
        conda list
        echo "---------------------------------------------------------------"
        echo "samtools: $(samtools --version | head -n1)"
    }


    export PATH="/opt/conda/bin:/usr/local/bin:/usr/bin:/bin:"
    unset CONDA_DEFAULT_ENV
    export ANACONDA_HOME=/opt/conda


    arg="${1:-none}"

    case "$arg" in
        none) usage; exit 1;;
        help) usage; exit 0;;
        list) tools; exit 0;;
        # just try to execute it then
        *)    $@;;
    esac


%test
    echo "This is the test code running" | sed -e 's/code //'
    false || true # This will fail without the double pipe
    true
