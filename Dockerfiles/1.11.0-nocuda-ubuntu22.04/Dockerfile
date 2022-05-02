FROM ubuntu:22.04
ENV PATH="/root/miniconda3/bin:${PATH}"
ARG PATH="/root/miniconda3/bin:${PATH}"

# создаем рабочую папку
RUN mkdir /deep-learning-school
WORKDIR /deep-learning-school

RUN apt-get update && apt-get upgrade && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

RUN curl -fSsl \
    https://repo.anaconda.com/miniconda/Miniconda3-py39_4.11.0-Linux-x86_64.sh  > ./Miniconda3-py39_4.11.0-Linux-x86_64.sh\
    && mkdir /root/.conda \
    && bash Miniconda3-py39_4.11.0-Linux-x86_64.sh -b \
    && rm -f Miniconda3-py39_4.11.0-Linux-x86_64.sh \
    && conda update -n base -c defaults conda 

# RUN conda install pytorch torchvision torchaudio cpuonly -c pytorch
COPY environment.yaml /open-ml-course/environment.yaml
RUN conda env update -n base -f environment.yaml \
    && conda list

# запускаем jupyter
CMD ["jupyter", "notebook", "--port=8888", "--no-browser", "--ip=0.0.0.0", "--allow-root", "--NotebookApp.token=''", "--NotebookApp.password=''"]
