# This Dockerfile aims to have at least one of everything, to exercise the
# comprehensiveness of Dockerfile feature support.
#
# FIXME: That focus is a bit out of date. I think really what is here is the
# ways we want to exercise ch-grow in ways we care about the resulting image.
# Exercises where we don't care are in test/build/50_dockerfile.bats. But, I
# don't want to do the refactoring right now.
#
# See: https://docs.docker.com/engine/reference/builder
#
# ch-test-scope: standard
# ch-test-builder-include: ch-grow

# Use a moderately complex image reference.
FROM registry-1.docker.io:443/library/alpine:3.9 AS stage1

# FIXME: These should maybe be moved into Dockerfile.argenv?
ENV chse_1 value1 with spaces and trailing space 
ENV chse_2 "value2"
ENV chse_2a chse2: ${chse_2}
ENV chse_2b="chse2: ${chse_2}"
ENV chse_3 \"value3\"
ENV chse_4=value4 chse_5="value5 foo" chse_7=\"value7\"
#ENV chse_4=value4 chse_5="value5 foo" chse_6=value6\ foo chse_7=\"value7\"

RUN pwd
WORKDIR /usr/local/src
RUN pwd
RUN ls --color=no -lh

RUN apk add --no-cache bc
RUN ["echo", "hello \n${chse_2} \${chse_2} ${NOTSET}"]
# should print:
# a -${chse_2}- b -value2- c -c- d -d-
RUN echo 'a -${chse_2}-' "b -${chse_2}-" "c -${NOTSET:-c}-" "d -${chse_2:+d}-"
RUN env

# WORKDIR. See test/run/ch-grow.bats where we validate this all worked OK.
# FIXME: test with variable
#
# filesystem root
WORKDIR /
RUN mkdir workdir
# absolute path, no mkdir
WORKDIR /workdir
RUN touch file
# absolute path, mkdir
RUN mkdir /workdir/abs2
WORKDIR /workdir/abs2
RUN touch file
# relative path, no mkdir
WORKDIR rel1
RUN touch file1
# relative path, 2nd level, no mkdir
WORKDIR rel2
RUN touch file
# relative path, parent dir, no mkdir
WORKDIR ..
RUN touch file2
# results
RUN ls -R /workdir

# TODO:
# comment with trailing backslash (line continuation does not work in comments)

