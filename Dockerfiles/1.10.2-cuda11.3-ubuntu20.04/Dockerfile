FROM nvidia/cuda:11.6.2-base-ubuntu20.04

RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
 && rm -rf /var/lib/apt/lists/*
 
# создаем рабочую папку
RUN mkdir /app
WORKDIR /app
 
# Create a non-root user and switch to it
RUN adduser --disabled-password --gecos '' --shell /bin/bash user \
 && chown -R user:user /app
RUN echo "user ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/90-user
USER user

# All users can use /home/user as their home directory
ENV HOME=/home/user
RUN mkdir $HOME/.cache $HOME/.config \
 && chmod -R 777 $HOME

# Set up the Conda environment
ENV CONDA_AUTO_UPDATE_CONDA=false \
    PATH=$HOME/miniconda/bin:$PATH
COPY environment.yml /app/environment.yml
    
RUN curl -sLo ~/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-Linux-x86_64.sh \
 && chmod +x ~/miniconda.sh \
 && ~/miniconda.sh -b -p ~/miniconda \
 && rm ~/miniconda.sh \
 && conda env update -n base -f /app/environment.yml \
 && rm /app/environment.yml \
 && conda clean -ya    

# Make RUN commands use the new environment:
SHELL ["/bin/bash", "-c"]
RUN conda init
RUN echo "conda activate base" >> ~/.bashrc
SHELL ["/bin/bash", "--login", "-c"]

# запускаем jupyter
ENTRYPOINT ["conda", "run", "jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''"]
