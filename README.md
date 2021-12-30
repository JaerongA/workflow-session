# Workflow for lab, subject and session management

This directory provides an example workflow to save the information related to lab, subject, and session metadata data management, using the following datajoint elements
+ [element-lab](https://github.com/datajoint/element-lab)
+ [element-animal](https://github.com/datajoint/element-animal)
+ [element-session](https://github.com/datajoint/element-session)

This repository provides demonstrations for:
Setting up a workflow using different elements (see [pipeline.py](workflow_session/pipeline.py))

## Workflow architecture
The lab and experiment subject management workflow presented here uses components from two DataJoint elements (element-lab, element-animal and element-session) assembled together into a functional workflow.

### element-lab

element-lab is used for all lab-general metadata, including personnel and projects. Many fields are optional, but may help for keeping track of lab members for multi-site projects or exporting data for publication.

![lab](images/lab_diagram.svg)

### element-animal

element-animal contains two modules, `subject` and `genotyping`.

`subject` contains basic information of subjects.
![subject](images/subject_diagram2.svg)

`genotyping` is designed for labs that handle animal care and genetic information themselves, which is optional.
![genotyping](images/genotyping_diagram2.svg)

### element-session
`session` is designed to handle metadata related to data collection, including collection date-time, file paths, and notes. Most workflows will include element-session as a starting point for further data entry.
![session](images/session_diagram2.png)

### This workflow
This workflow serves as an example of the upstream part of a typical data workflow, for examples using these elements in tandem with other data collection modalities, refer to:

+ [workflow-array-ephys](https://github.com/datajoint/workflow-array-ephys)
+ [workflow-calcium-imaging](https://github.com/datajoint/workflow-calcium-imaging)


## Installation instructions

### Step 1 - Clone this repository

+ Launch a new terminal and change directory to where you want to clone the repository
    ```
    cd C:/Projects
    ```
+ Clone the repository
    ```
    git clone https://github.com/datajoint/workflow-session
    ```
+ Change directory to `workflow-session`
    ```
    cd workflow-session
    ```

### Step 2 - Setup a virtual environment
It is highly recommended (though not strictly required) to create a virtual environment to run the pipeline. This can be done with either `virtualenv` or `conda`

+ For `virtualenv`:

    + If not yet installed, run `pip install --user virtualenv`

    + To create a new virtual environment named `venv`:
        ```
        virtualenv venv
        ```

    + To activated the virtual environment:
        + On Windows:
            ```
            .\venv\Scripts\activate
            ```

        + On Linux/macOS:
            ```
            source venv/bin/activate
            ```
+ For `conda`:
    + If not yet installed, run `pip install --user conda`

    + To create a new virtual environment named `venv`:
        ```
        conda create --name venv python=3.8
        ```

    + To activated the virtual environment:
        + On Windows:
            ```
            activate venv
            ```

        + On Linux/macOS:
            ```
            source activate venv
            ```

### Step 3 - Install this repository

From the root of the cloned repository directory:
    ```
    pip install -e .
    ```

Note: the `-e` flag will install this repository in editable mode,
in case you'd like to to modify the code (e.g. the `pipeline.py` or `paths.py` scripts).
If no such modification required, using `pip install .` is sufficient.


### Step 4 - Jupyter Notebook
+ Register an IPython kernel with Jupyter
    ```
    ipython kernel install --name=workflow-session
    ```

### Step 5 - Configure the `dj_local_conf.json`

At the root of the repository folder,
create a new file `dj_local_conf.json` with the following template:

```json
{
  "database.host": "<hostname>",
  "database.user": "<username>",
  "database.password": "<password>",
  "loglevel": "INFO",
  "safemode": true,
  "display.limit": 7,
  "display.width": 14,
  "display.show_tuple_count": true,
  "custom": {
      "database.prefix": "<neuro_>",
}
```

+ Specify database's `hostname`, `username`, and `password` according to the database you plan to use (see [set-up instructions here](https://tutorials.datajoint.io/setting-up/get-database.html)).

+ Specify a `database.prefix` to create the schemas.


### Installation complete

+ At this point the setup of this workflow is complete.


## Interacting with the DataJoint pipeline and exploring data

+ [Connect to database](https://tutorials.datajoint.io/setting-up/get-database.html)

+ Import tables
    ```
    from workflow_session.pipeline import *
    ```
    This will create all tables defined in the elements in the database server.

+ Preview the tables created by calling the classes, for example:
    ```
    lab.Lab()
    subject.Subject()
    session.Session()
    genotyping.GenotypingTest()
    ```

+ If required to drop all schemas, the following is the dependency order.
    ```
    from workflow_session.pipeline import *

    genotyping.schema.drop()
    session.schema.drop()
    subject.schema.drop()
    lab.schema.drop()
    ```

+ For a more in-depth exploration of the tables created, please refer to the example [notebooks](notebooks/1_Explore_Workflow.ipynb).


## Insert into Manual and Lookup tables with Graphical User Interface DataJoint Labbook

DataJoint also provides a Graphical User Interface [DataJoint Labbook](https://github.com/datajoint/datajoint-labbook) to support manual data insertions into DataJoint workflows.

![DataJoint Labbook preview](images/DataJoint_Labbook.png)

Please refer to the [DataJoint Labbook page](https://github.com/datajoint/datajoint-labbook) for instructions to set it up.

## Development mode installation

This method allows you to modify the source code for `workflow-calcium-imaging`, `element-calcium-imaging`, `element-session`, `element-animal`, and `element-lab`.

+ Launch a new terminal and change directory to where you want to clone the repositories
    ```
    cd C:/Projects
    ```
+ Clone the repositories
    ```
    git clone https://github.com/datajoint/element-lab
    git clone https://github.com/datajoint/element-animal
    git clone https://github.com/datajoint/element-session
    git clone https://github.com/datajoint/workflow-session
    ```
+ Install each package with the `-e` option
    ```
    pip install -e ./workflow-session
    pip install -e ./element-session
    pip install -e ./element-lab
    pip install -e ./element-animal
    ```
