/---Pipelines---/

CWL-common workflow language(интегрирован с docker)

Явное описание графа анализа(в центре фокуса программы)

Snakemake(python+make)

Неявное описание графа анализа(в центре фокуса оказываются данные)

Языки для pipeline:

- BDS(Big Data Script)

- CWL(Common Workflow Management)

- WDL(Workflow definition language)

- Perl,Python,Ruby,Scala,etc..

NIH syndrome

pip3 install snakemake

pip3 install 'snakemake<=3.13.3'

sudo apt install python3-dev # пакет python3-dev

рекомендуем установить graphviz, который удобно использовать для визуализации графа пайплайна.

[http://www.graphviz.org](http://www.graphviz.org)

можно не ставить graphviz, а пользоваться онлайн инструментом: webgraphviz.com

snakemake -s Snakefile --dag | dot -Tsvg > pipeline.svg # сохранить граф пайплайна, описанного в файле Snakefile

snakemake -s Snakefile --dag | dot -Tsvg | display # вывести на экран

snakemake -F # launch force

snakemake # вызовет файл Snakefile(название по умолчанию)

The input of rule all represents the final output of your pipeline

rule copy:

    input: "out.txt"

    output: "copy.txt"

    shell: "cp{input}{output}"

rule generate:

    output: "out.txt"

    shell: "touch{output}"

/---Create Snakemake pipeline---/

Вам необходимо создать пайплайн c использованием Snakemake. Задача пайплайна состоит в подсчёте количества слов в исходном файле и записи полученного результата в выходной файл. Пайплайн должен работать экономно, в том случае, если  файл с результатами уже получен, подсчет не должен производиться повторно.

Файл input/input, содержащий текст произвольной длины, например:

Файл output/output, содержащий число – количество слов во входном файле, например:

Пайплайн Snakemake (необходимо оформить в виде файла c названием Snakefile).

Проверка решения осуществляется локально на любом компьютере, на котором можно запустить Docker.

Для проверки решения перейдите в директорию, в которой находится файл с вашим решением, и запустите следующую команду:

docker run --rm -v $(pwd)/Snakefile:/mnt/Snakefile parseq/stepik-word-count-snakemake

rule copy:

    input: "input/input"

    output: "output/output"

    shell: "python3 count.py {input}{output}"

# run snakemake in docker

docker run -it --rm --name graphviz_container -v $(pwd)/stepik_task:/home graphviz_image

============Graphiz Docker============

=======Dockerfile======

FROM alpine:3.4 #

MAINTAINER Rus

RUN apk add --update --no-cache \

           graphviz \

           ttf-freefont

RUN apk add --no-cache python3 && \

    python3 -m ensurepip && \

    rm -r /usr/lib/python*/ensurepip && \

    pip3 install --upgrade pip setuptools && \

    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \

    if [[ ! -e /usr/bin/python ]]; then ln -sf /usr/bin/python3 /usr/bin/python; fi && \

    rm -r /root/.cache

RUN pip3 install 'snakemake<=3.13.3'

=====================

FROM ubuntu:14.04 #

MAINTAINER Rus

RUN apt-get update && apt-get install -y --no-install-recommends \

           graphviz \

           ttf-freefont \

              python3.5 \

           python3-pip \

           && \

    apt-get clean && \

    rm -rf /var/lib/apt/lists/*

RUN pip3 install snakemake

=========================

docker build -t graphviz_image graphviz_docker/

docker run -it --rm --name graphviz_container -v $(pwd)/graphviz_folder:/home graphviz_image

1. Create file <filename.gv>

digraph HelloWorld {

  "Hello" -> "World";

}

2. dot -Tpng <filename.gv> -o example.png

pip3 install snakemake

snakemake -s Snakefile --dag | dot -Tsvg > pipeline.svg

# Костыль. Не разобрался как в Dockerfile правильно передать установку python package. Поэтому создаю образ без pip, создаю контейнер -> устанавливаю пакеты -> из контейнера создаю образ. Хотя нужно попробовать добавить в Dockerfile версию 'snakemake<=3.13.3'

docker commit <container_name> <image_name>

=======================

Часто возникает необходимость обработать большое количество файлов, находящихся в некоторой папке, при этом указывать имена файлов явно оказывается неудобно.

Эта ситуация создает трудности и для оригинального Make и для Snakemake, так как пайплайн в обоих случаях строится вокруг результатов выполнения задачи. Таким образом, для решения проблем такого рода необходимо использовать средства языка для получения списка входных файлов, согласно соглашению о наименовании файлов генерировать имена выходных файлов, и этот список подавать в качестве целей в пайплайне.

snakemakes используют wildcards({this is wildcard})- сопоставляют входным файлам выходные

rule all:

   input:["out/1","out/2","out/3"]

   output: ".status"

   shell: touch{output}

rule copy:

    input: "in/{file}"

    output: "out/{file}"

    shell: "cp{input}{output}"

=======================

вместо shell в Snakemake в качестве команды можно использовать код Python

rule NAME:

    input: "path/to/inputfile", "path/to/other/inputfile"

    output: "path/to/outputfile", somename = "path/to/another/outputfile"

    run:

        for f in input:

            ...

            with open(output[0], "w") as out:

                out.write(...)

        with open(output.somename, "w") as out:

            out.write(...)

---task---

Необходимо создать пайплайн Snakemake, который для некоторого набора файлов входной директории создает файлы в выходной директории.

glob_wildcards analog og glob.glob

expand - merge list IDS with string. {}.txt ['sample1','sample2'] => sample1.txt, sample2.txt

IDS, = glob_wildcards("input/{id,[^\.]\w+\.*\w*}")

rule all:

    input: expand("output/{id}", id=IDS)

rule copy:

    input: "input/{id}"

    output: "output/{id}"

    shell: "echo Hello $(cat {input})! > {output}"