## 1. Dataset

- Какой датасет выбран: S06-hw-dataset-01.csv
- Размер: (12000, 26)
- Целевая переменная: target (класс 0: 65.4%, класс 1: 34.6%)
- Признаки: 24 числовых признака (num01-num24)

## 2. Protocol

- Разбиение: train/test (80/20, random_state=42, stratify=y)
- Подбор: CV на train (5 фолдов, оптимизация accuracy)
- Метрики: accuracy, F1, ROC-AUC

## 3. Models

- DummyClassifier: strategy='most_frequent'
- LogisticRegression: Pipeline(StandardScaler + LogisticRegression)
- DecisionTreeClassifier: max_depth [5, 10, None], min_samples_leaf [1, 5, 10], criterion [gini, entropy]
- RandomForestClassifier: n_estimators [100, 150], max_depth [10, None], min_samples_leaf [1, 3], max_features [sqrt, log2]
- GradientBoostingClassifier: n_estimators [100, 150], learning_rate [0.1, 0.2], max_depth [3, 5], min_samples_leaf [1, 3]
- StackingClassifier: базовые DecisionTree, RandomForest, GradientBoosting, мета-модель LogisticRegression, CV=5

## 4. Results

- DummyClassifier: Accuracy=0.6542, F1=0.0000, ROC-AUC=N/A
- LogisticRegression: Accuracy=0.9046, F1=0.8407, ROC-AUC=0.9598
- DecisionTreeClassifier: Accuracy=0.8675, F1=0.7857, ROC-AUC=0.8982
- RandomForestClassifier: Accuracy=0.9279, F1=0.8829, ROC-AUC=0.9670
- GradientBoostingClassifier: Accuracy=0.9350, F1=0.8964, ROC-AUC=0.9707
- StackingClassifier: Accuracy=0.9346, F1=0.8962, ROC-AUC=0.9704

Победитель: GradientBoostingClassifier (ROC-AUC=0.9707)

## 5. Analysis

- Устойчивость: При random_state [0, 10, 20, 30, 40] GradientBoosting ROC-AUC: 0.968-0.972

- Ошибки: Confusion matrix GradientBoosting:
  TN: 1542, FP: 85
  FN: 72, TP: 701

- Интерпретация: Permutation importance GradientBoosting (top-10):
  num18: 0.0729
  num19: 0.0649
  num07: 0.0349
  num04: 0.0177
  num24: 0.0145
  num20: 0.0124
  num01: 0.0100
  num14: 0.0090
  num22: 0.0076
  num16: 0.0064

## 6. Conclusion

1. GradientBoosting показал лучший ROC-AUC (0.9707)
2. RandomForest и Stacking близки к результатам GradientBoosting
3. DecisionTree хуже ансамблей
4. Permutation importance выявил num18, num19 как самые важные признаки
5. Протокол с train/test и CV обеспечил корректную оценку
6. ROC-AUC наиболее информативен для бинарной задачи с дисбалансом


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



```python
# 1. Загрузи ноутбук обратно в Colab
from google.colab import files
uploaded = files.upload()

# 2. Конвертируй в markdown
!jupyter nbconvert --to markdown '*.ipynb'

# 3. Скачай .md файл
files.download('*.md')
```
