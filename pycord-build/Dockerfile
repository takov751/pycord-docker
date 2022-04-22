FROM python:alpine AS builder

RUN apk add libffi-dev \
            build-base \
            py3-pynacl \
            python3-dev \
            git && \
    git clone https://github.com/Pycord-Development/pycord && \
    cd ./pycord && python3 -m venv /venv && \
    /venv/bin/python3 -m pip install -U .

FROM python:alpine AS noaudio
#RUN apk add --no-cache ffmpeg && rm -rf /var/cache/apk/*
COPY --from=builder /venv /venv
COPY ./run.py /run.py
CMD ["/venv/bin/python3", "-u", "/run.py"]


FROM builder AS audiobuilder

RUN cd /pycord && \
    /venv/bin/python3 -m pip install -U .[voice]

FROM python:alpine AS audio

RUN apk add --no-cache ffmpeg && \
    rm -rf /var/cache/apk/*
COPY --from=audiobuilder /venv /venv
COPY ./run_audio.py /run.py
CMD ["/venv/bin/python3", "-u", "/run.py"]

