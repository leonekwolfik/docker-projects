# Dockerfile
FROM python:3.11

WORKDIR /app

COPY requirements.txt /app
RUN pip install --no-cache-dir -r requirements.txt

COPY streamlit_app.py /app

EXPOSE 8501

CMD ["streamlit", "run", "streamlit_app.py", "--server.port=8501"]
