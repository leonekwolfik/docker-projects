# Generative Models by Stability AI Dockerfile

# Wybieramy obraz bazowy z Pythonem 3.10
FROM python:3.10

# Ustawienie zmiennej środowiskowej z tokenem Hugging Face
ENV HF_TOKEN=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

# Klonujemy repozytorium generative-models z GitHuba
RUN git clone https://github.com/Stability-AI/generative-models.git
WORKDIR /generative-models

# Tworzymy wirtualne środowisko i instalujemy wymagane pakiety
RUN python3 -m venv .pt2
RUN /bin/bash -c "source .pt2/bin/activate && pip3 install -r requirements/pt2.txt"

# Instalujemy sgm, sdata i streamlit
RUN /bin/bash -c "pip3 install ." && \
    /bin/bash -c "pip3 install -e git+https://github.com/Stability-AI/datapipelines.git@main#egg=sdata" && \
    /bin/bash -c "pip3 install streamlit"

# Pobieramy wagi za pomocą curl i umieszczamy je w katalogu checkpoints/
RUN mkdir checkpoints && \
    curl -H "Authorization: Bearer $HF_TOKEN" -o checkpoints/stable-diffusion-xl-base-0.9.tar.gz https://huggingface.co/stabilityai/stable-diffusion-xl-base-0.9/resolve/main/checkpoint.tar.gz && \
    curl -H "Authorization: Bearer $HF_TOKEN" -o checkpoints/stable-diffusion-xl-refiner-0.9.tar.gz https://huggingface.co/stabilityai/stable-diffusion-xl-refiner-0.9/resolve/main/checkpoint.tar.gz

# Oznaczamy port 3000 do użycia
EXPOSE 3000

# Komenda startowa do uruchomienia aplikacji
CMD ["streamlit", "run", "scripts/demo/sampling.py", "--server.port", "3000"]
