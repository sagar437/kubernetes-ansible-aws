FROM python:3.7.2-stretch
WORKDIR /opt/project
COPY . /opt/project/
RUN pip3 install --upgrade pip==19.3.1
RUN pip install -r requirements.txt
EXPOSE 8000
CMD ["gunicorn", "-b", "0.0.0.0:8000", "server:app"]
