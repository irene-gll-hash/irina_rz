## 1. Datasets

Вы выбрали 3 датасета из 4 (перечислите): S07-hw-dataset-01.csv, S07-hw-dataset-02.csv, S07-hw-dataset-03.csv

### 1.1 Dataset A

- Файл: `S07-hw-dataset-01.csv`
- Размер: (1000, 6)
- Признаки: 6 числовых
- Пропуски: нет
- "Подлости" датасета: разные шкалы признаков

### 1.2 Dataset B

- Файл: `S07-hw-dataset-02.csv`
- Размер: (1000, 4)
- Признаки: 4 числовых
- Пропуски: нет
- "Подлости" датасета: нелинейная структура, выбросы, шумовой признак

### 1.3 Dataset C

- Файл: `S07-hw-dataset-03.csv`
- Размер: (1000, 4)
- Признаки: 4 числовых
- Пропуски: нет
- "Подлости" датасета: кластеры разной плотности, фоновый шум

## 2. Protocol

- Препроцессинг: StandardScaler для всех числовых признаков, SimpleImputer(strategy='median') для пропусков (хотя их не было)
- Поиск гиперпараметров:
  - KMeans: k от 2 до 10
  - DBSCAN: eps от 0.1 до 2.0, min_samples=5 фиксировано
  - Руководствовались silhouette score и PCA визуализацией
- Метрики: silhouette_score, davies_bouldin_score, calinski_harabasz_score. Для DBSCAN метрики считались только на non-noise точках
- Визуализация: PCA(2D) с random_state=42

## 3. Models

Для каждого датасета:
- KMeans: k от 2 до 10, random_state=42, n_init=10
- DBSCAN: eps от 0.1 до 2.0, min_samples=5

## 4. Results

### 4.1 Dataset A

- Лучший метод и параметры: KMeans с k=2
- Метрики (silhouette / DB / CH): 0.522 / 0.753 / 2551.2
- Если был DBSCAN: доля шума 3.4%
- Коротко: KMeans лучше из-за сферических кластеров

### 4.2 Dataset B

- Лучший метод и параметры: DBSCAN с eps=0.5
- Метрики (silhouette / DB / CH): DBSCAN 0.458 / 0.923 / 815.3, KMeans 0.307 / 1.432 / 532.7
- Если был DBSCAN: доля шума 4.5%
- Коротко: DBSCAN справился с нелинейной структурой

### 4.3 Dataset C

- Лучший метод и параметры: DBSCAN с eps=0.3
- Метрики (silhouette / DB / CH): DBSCAN 0.421 / 0.874 / 689.4, KMeans 0.316 / 1.386 / 541.2
- Если был DBSCAN: доля шума 2.2%
- Коротко: DBSCAN отделил плотные ядра от фона

## 5. Analysis

### 5.1 Сравнение алгоритмов (важные наблюдения)

- KMeans "ломается" на нелинейных данных и при разной плотности кластеров, так как ищет сферические кластеры
- DBSCAN выигрывает при произвольной форме кластеров и разной плотности
- Сильнее всего влияло: масштабирование для KMeans, наличие выбросов и разная плотность

### 5.2 Устойчивость (обязательно для одного датасета)

- Какую проверку устойчивости делали: 5 запусков KMeans на датасете B с разными random_state (42, 123, 777, 13, 99)
- Что получилось: средний ARI=0.970, минимальный ARI=0.941
- Вывод: высокая устойчивость благодаря четким кластерам после масштабирования

### 5.3 Интерпретация кластеров

- Как вы интерпретировали кластеры: через средние значения признаков в каждом кластере
- Выводы: кластеры соответствуют скоплениям в пространстве признаков, PCA визуализация подтверждает качество разделения

## 6. Conclusion

1. Масштабирование критически важно для KMeans
2. KMeans работает только со сферическими кластерами одинаковой плотности
3. DBSCAN лучше для нелинейных данных и кластеров разной плотности
4. Метрики для DBSCAN нужно считать только на non-noise точках
5. Визуализация (PCA) помогает оценить качество кластеризации
6. KMeans показывает высокую устойчивость на четких данных


```python

```

    [NbConvertApp] WARNING | pattern '6report.ipynb' matched no files
    This application is used to convert notebook files (*.ipynb)
            to various other formats.
    
            WARNING: THE COMMANDLINE INTERFACE MAY CHANGE IN FUTURE RELEASES.
    
    Options
    =======
    The options below are convenience aliases to configurable class-options,
    as listed in the "Equivalent to" description-line of the aliases.
    To see all configurable class-options for some <cmd>, use:
        <cmd> --help-all
    
    --debug
        set log level to logging.DEBUG (maximize logging output)
        Equivalent to: [--Application.log_level=10]
    --show-config
        Show the application's configuration (human-readable format)
        Equivalent to: [--Application.show_config=True]
    --show-config-json
        Show the application's configuration (json format)
        Equivalent to: [--Application.show_config_json=True]
    --generate-config
        generate default config file
        Equivalent to: [--JupyterApp.generate_config=True]
    -y
        Answer yes to any questions instead of prompting.
        Equivalent to: [--JupyterApp.answer_yes=True]
    --execute
        Execute the notebook prior to export.
        Equivalent to: [--ExecutePreprocessor.enabled=True]
    --allow-errors
        Continue notebook execution even if one of the cells throws an error and include the error message in the cell output (the default behaviour is to abort conversion). This flag is only relevant if '--execute' was specified, too.
        Equivalent to: [--ExecutePreprocessor.allow_errors=True]
    --stdin
        read a single notebook file from stdin. Write the resulting notebook with default basename 'notebook.*'
        Equivalent to: [--NbConvertApp.from_stdin=True]
    --stdout
        Write notebook output to stdout instead of files.
        Equivalent to: [--NbConvertApp.writer_class=StdoutWriter]
    --inplace
        Run nbconvert in place, overwriting the existing notebook (only
                relevant when converting to notebook format)
        Equivalent to: [--NbConvertApp.use_output_suffix=False --NbConvertApp.export_format=notebook --FilesWriter.build_directory=]
    --clear-output
        Clear output of current file and save in place,
                overwriting the existing notebook.
        Equivalent to: [--NbConvertApp.use_output_suffix=False --NbConvertApp.export_format=notebook --FilesWriter.build_directory= --ClearOutputPreprocessor.enabled=True]
    --coalesce-streams
        Coalesce consecutive stdout and stderr outputs into one stream (within each cell).
        Equivalent to: [--NbConvertApp.use_output_suffix=False --NbConvertApp.export_format=notebook --FilesWriter.build_directory= --CoalesceStreamsPreprocessor.enabled=True]
    --no-prompt
        Exclude input and output prompts from converted document.
        Equivalent to: [--TemplateExporter.exclude_input_prompt=True --TemplateExporter.exclude_output_prompt=True]
    --no-input
        Exclude input cells and output prompts from converted document.
                This mode is ideal for generating code-free reports.
        Equivalent to: [--TemplateExporter.exclude_output_prompt=True --TemplateExporter.exclude_input=True --TemplateExporter.exclude_input_prompt=True]
    --allow-chromium-download
        Whether to allow downloading chromium if no suitable version is found on the system.
        Equivalent to: [--WebPDFExporter.allow_chromium_download=True]
    --disable-chromium-sandbox
        Disable chromium security sandbox when converting to PDF..
        Equivalent to: [--WebPDFExporter.disable_sandbox=True]
    --show-input
        Shows code input. This flag is only useful for dejavu users.
        Equivalent to: [--TemplateExporter.exclude_input=False]
    --embed-images
        Embed the images as base64 dataurls in the output. This flag is only useful for the HTML/WebPDF/Slides exports.
        Equivalent to: [--HTMLExporter.embed_images=True]
    --sanitize-html
        Whether the HTML in Markdown cells and cell outputs should be sanitized..
        Equivalent to: [--HTMLExporter.sanitize_html=True]
    --log-level=<Enum>
        Set the log level by value or name.
        Choices: any of [0, 10, 20, 30, 40, 50, 'DEBUG', 'INFO', 'WARN', 'ERROR', 'CRITICAL']
        Default: 30
        Equivalent to: [--Application.log_level]
    --config=<Unicode>
        Full path of a config file.
        Default: ''
        Equivalent to: [--JupyterApp.config_file]
    --to=<Unicode>
        The export format to be used, either one of the built-in formats
                ['asciidoc', 'custom', 'html', 'latex', 'markdown', 'notebook', 'pdf', 'python', 'qtpdf', 'qtpng', 'rst', 'script', 'slides', 'webpdf']
                or a dotted object name that represents the import path for an
                ``Exporter`` class
        Default: ''
        Equivalent to: [--NbConvertApp.export_format]
    --template=<Unicode>
        Name of the template to use
        Default: ''
        Equivalent to: [--TemplateExporter.template_name]
    --template-file=<Unicode>
        Name of the template file to use
        Default: None
        Equivalent to: [--TemplateExporter.template_file]
    --theme=<Unicode>
        Template specific theme(e.g. the name of a JupyterLab CSS theme distributed
        as prebuilt extension for the lab template)
        Default: 'light'
        Equivalent to: [--HTMLExporter.theme]
    --sanitize_html=<Bool>
        Whether the HTML in Markdown cells and cell outputs should be sanitized.This
        should be set to True by nbviewer or similar tools.
        Default: False
        Equivalent to: [--HTMLExporter.sanitize_html]
    --writer=<DottedObjectName>
        Writer class used to write the
                                            results of the conversion
        Default: 'FilesWriter'
        Equivalent to: [--NbConvertApp.writer_class]
    --post=<DottedOrNone>
        PostProcessor class used to write the
                                            results of the conversion
        Default: ''
        Equivalent to: [--NbConvertApp.postprocessor_class]
    --output=<Unicode>
        Overwrite base name use for output files.
                    Supports pattern replacements '{notebook_name}'.
        Default: '{notebook_name}'
        Equivalent to: [--NbConvertApp.output_base]
    --output-dir=<Unicode>
        Directory to write output(s) to. Defaults
                                      to output to the directory of each notebook. To recover
                                      previous default behaviour (outputting to the current
                                      working directory) use . as the flag value.
        Default: ''
        Equivalent to: [--FilesWriter.build_directory]
    --reveal-prefix=<Unicode>
        The URL prefix for reveal.js (version 3.x).
                This defaults to the reveal CDN, but can be any url pointing to a copy
                of reveal.js.
                For speaker notes to work, this must be a relative path to a local
                copy of reveal.js: e.g., "reveal.js".
                If a relative path is given, it must be a subdirectory of the
                current directory (from which the server is run).
                See the usage documentation
                (https://nbconvert.readthedocs.io/en/latest/usage.html#reveal-js-html-slideshow)
                for more details.
        Default: ''
        Equivalent to: [--SlidesExporter.reveal_url_prefix]
    --nbformat=<Enum>
        The nbformat version to write.
                Use this to downgrade notebooks.
        Choices: any of [1, 2, 3, 4]
        Default: 4
        Equivalent to: [--NotebookExporter.nbformat_version]
    
    Examples
    --------
    
        The simplest way to use nbconvert is
    
                > jupyter nbconvert mynotebook.ipynb --to html
    
                Options include ['asciidoc', 'custom', 'html', 'latex', 'markdown', 'notebook', 'pdf', 'python', 'qtpdf', 'qtpng', 'rst', 'script', 'slides', 'webpdf'].
    
                > jupyter nbconvert --to latex mynotebook.ipynb
    
                Both HTML and LaTeX support multiple output templates. LaTeX includes
                'base', 'article' and 'report'.  HTML includes 'basic', 'lab' and
                'classic'. You can specify the flavor of the format used.
    
                > jupyter nbconvert --to html --template lab mynotebook.ipynb
    
                You can also pipe the output to stdout, rather than a file
    
                > jupyter nbconvert mynotebook.ipynb --stdout
    
                PDF is generated via latex
    
                > jupyter nbconvert mynotebook.ipynb --to pdf
    
                You can get (and serve) a Reveal.js-powered slideshow
    
                > jupyter nbconvert myslides.ipynb --to slides --post serve
    
                Multiple notebooks can be given at the command line in a couple of
                different ways:
    
                > jupyter nbconvert notebook*.ipynb
                > jupyter nbconvert notebook1.ipynb notebook2.ipynb
    
                or you can specify the notebooks list in a config file, containing::
    
                    c.NbConvertApp.notebooks = ["my_notebook.ipynb"]
    
                > jupyter nbconvert --config mycfg.py
    
    To see all available configurables, use `--help-all`.
    



    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    /tmp/ipython-input-1778501166.py in <cell line: 0>()
          1 get_ipython().system('jupyter nbconvert --to markdown 6report.ipynb')
          2 from google.colab import files
    ----> 3 files.download('6report.md')
    

    /usr/local/lib/python3.12/dist-packages/google/colab/files.py in download(filename)
        228   if not _os.path.exists(filename):
        229     msg = 'Cannot find file: {}'.format(filename)
    --> 230     raise FileNotFoundError(msg)  # pylint: disable=undefined-variable
        231 
        232   comm_manager = _IPython.get_ipython().kernel.comm_manager


    FileNotFoundError: Cannot find file: 6report.md

