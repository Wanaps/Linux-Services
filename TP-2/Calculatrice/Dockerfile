FROM ubuntu
RUN apt-get update
RUN apt-get install -y python3 python3-venv
RUN python3 -m venv /venv
ENV PATH="/venv/bin:$PATH"
RUN /venv/bin/pip install emoji
RUN python3 -m pip install emoji
WORKDIR /app
COPY server.py /app
ENTRYPOINT ["python3", "server.py"]