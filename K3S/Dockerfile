FROM python:3.7
COPY . /app
WORKDIR /app
RUN pip3 install pipenv
RUN pipenv lock --requirements > requirements.txt
RUN pip3 install -r requirements.txt
CMD ["python3", "main.py"]