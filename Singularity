Bootstrap: docker
From: bioconductor/release_core2

%environment
export PATH="/opt/anaconda2/bin:$PATH"

%runscript

   #"I can put here whatever I want to happen by default when the user runs the container"
   cat << EOF
To execute a binary inside the container do "singularity exec /path/to/container.img binary-name"
EOF

%post

	echo "Here we are installing software and other dependencies for the container!"

	echo "Installing pip and basic dependencies"
	apt-get update
	apt-get install -y curl wget libboost-all-dev python-pip libudunits2-dev
	
	# Install miniconda
	wget https://repo.continuum.io/miniconda/Miniconda2-latest-Linux-x86_64.sh
	bash Miniconda2-latest-Linux-x86_64.sh -b -p /opt/anaconda2
	
	# Install samtools and samblaster
	conda install -c bioconda samtools=1.9 bowtie=1.2.2 fastqc=0.11.7 macs2=2.1.1.20160309 \
	deeptools=3.1.1 multiqc=1.6a0 samblaster=0.1.24 wiggletools=1.2.2 fastp=0.19.3
	
	# R packages and bioconductor
	R --slave -e "source('https://bioconductor.org/biocLite.R'); \
                      biocLite('ChIPseeker')"
 	R --slave -e "source('https://bioconductor.org/biocLite.R'); \
                      biocLite('TxDb.Mmusculus.UCSC.mm9.knownGene')"                     
	R --slave -e 'install.packages(c("ggplot2", "data.table", "RColorBrewer", "devtools", "snow"), repos="https://cloud.r-project.org/")'
	R --slave -e "library(devtools); \
	                  devtools::install_github('hms-dbmi/spp', build_vignettes = FALSE)"

#%apprun bowtie
#%apprun samblaster
#%apprun deeptools
#%apprun samtools
#%apprun fastp
