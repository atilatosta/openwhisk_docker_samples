FROM openwhisk/python3action

# lapack-dev is available in community repo.
RUN echo "http://dl-4.alpinelinux.org/alpine/edge/community" >> /etc/apk/repositories

# add package build dependencies
RUN apk add --no-cache \
        g++ \
        lapack-dev \
        gfortran

# add the main file to image
ADD __main__.py ./

# add python packages
RUN pip install \
    watson-developer-cloud
