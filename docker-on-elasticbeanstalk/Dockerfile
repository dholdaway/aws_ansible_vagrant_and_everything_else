FROM python:2.7
EXPOSE 9000
COPY requirements.txt /
COPY main.py /
RUN ["pip", "install", "-r", "requirements.txt"]
CMD ["uwsgi", "--socket", "0.0.0.0:9000", "--protocol", "http", "-w", "main:app"]