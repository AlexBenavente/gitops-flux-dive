FROM python:3.10-slim-bullseye
WORKDIR /python
COPY ./python .
RUN pip install -r requirements.txt
EXPOSE 7000
CMD python app.py