# Generative Models by Stability AI Dockerfile

# Wybieramy obraz bazowy z Pythonem 3.10
FROM nvidia/cuda:11.7.1-cudnn8-runtime-ubuntu22.04

# Ustawienie zmiennej środowiskowej z tokenem Hugging Face
ENV HF_TOKEN=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
ARG DEBIAN_FRONTEND=noninteractive

# Install GIT
RUN apt-get update
RUN apt-get install -y git

# Klonujemy repozytorium generative-models z GitHuba
# RUN git clone https://github.com/Stability-AI/generative-models.git
COPY /generative-models/ /generative-models/

# Kopiujemy wagi
COPY /weights/stable-diffusion-xl-base-1.0/*.safetensors /generative-models/checkpoints/
COPY /weights/stable-diffusion-xl-refiner-1.0/*.safetensors /generative-models/checkpoints/
WORKDIR /generative-models

# Instalujemy brakujące zależności
RUN apt-get update && \
    apt-get install -y libgl1-mesa-glx libtiff-dev cuda-toolkit-11-7 git

# Install Python 3.10 and required tools
RUN apt-get update && \
    apt-get install -y software-properties-common curl && \
    add-apt-repository -y ppa:deadsnakes/ppa && \
    apt-get update && \
    apt-get install -y python3.10 python3.10-dev python3.10-distutils && \
    ln -s /usr/bin/python3.10 /usr/local/bin/python && \
    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && \
    python3.10 get-pip.py && \
    rm get-pip.py



# Instalujemy zależności
RUN pip3 install -r requirements/pt2.txt
RUN pip3 install streamlit
RUN pip3 install -e .
RUN pip3 install -e git+https://github.com/Stability-AI/datapipelines.git@main#egg=sdata
RUN pip3 install -e .


# Oznaczamy port 3000 do użycia
EXPOSE 8501

# Komenda startowa do uruchomienia aplikacji
CMD ["streamlit", "run", "scripts/demo/sampling.py", "--server.port=8501"]
