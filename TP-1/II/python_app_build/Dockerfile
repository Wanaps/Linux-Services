FROM debian
RUN apt update
RUN apt install -y python3 python3-venv
RUN python3 -m venv /venv
ENV PATH="/venv/bin:$PATH"
RUN /venv/bin/pip install emoji
RUN python3 -m pip install emoji
WORKDIR /python_app_build/
COPY app.py /home/python_app_build/app.py
ENTRYPOINT ["python3", "app.py"]
