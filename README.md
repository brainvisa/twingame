# twingame

Twins game in [Anatomist display](https://brainvisa.info/web/anatomist.html): find the twins by looking at their brains

The programs displays 3D brains. They are pairs of twins brains. The user has to pair them correctly.

A config file tells the program where are the data files for brains meshes, sulci etc, who are the twins etc. This file has to be built, it depends on the dataset.

A dataset of preprocessed twins has to be installed. The data (T1-weighted MRI images) should have been processed using [Morphologist](https://brainvisa.info/web/morphologist.html)

Iy you are using a Morphologist-preprocessed version of the HCP dataset, then it is possible to reuse one of the conig files shipped with the program, with minimal modifications.


## Installation and setup for the HCP dataset

For a diffferent dataset, you would have to rebuild a new config file describing each subject and twin pair in the dataset. This is not documented up to now (although not very complex).

Works only on Linux systems (or in Windows/WSL2)

* install Anatomist using [Pixi](https://pixi.prefix.dev/latest/):

    ```
    # install pixi
    curl -fsSL https://pixi.sh/install.sh | sh
    # install anatomist
    mkdir $HOME/anatomist
    cd $HOME/anatomist
    pixi init -c https://brainvisa.info/neuro-forge -c conda-forge
    pixi add anatomist
    # make conventient links to enter pixi environment (optional)
    pixi run bv_update_bin_links
    ```

* Clone this repository:

    ```
    mkdir $HOME/twingame
    cd $HOME/twingame
    git clone https://github.com/brainvisa/twingame.git
    ```

* Copy the HCP config file at a convenient location:

    ```
    cp twingame/share/config/twin_config_hcp.json my_twin_config.json
    ````

* Edit it to set the path of your HCP dataset (once again, only for HCP), as in this example:

    ```
    {
        "twin_number": 6,
        "show_sulci": false,
        "dataset": {
            "directory": "/data/hcp/3T_morphologist",
            "metadata": {
                "center": "hcp",
                "acquisition": "BL",
                "graph_version": "3.1",
                "sulci_recognition_session": "deepcnn_auto",
                "under_ses": ""
            },
            "twins": {
                "twin_0000": [
                    "100307",
                    "255639"
                ],
                "twin_0001": [
                    "100408",
                    "705341"
                ],
    # ...
    ```

* run twingame inside the anatomist/pixi environment, specifying the config file:

    ```
    $HOME/anatomist/bin/bv bin/twingame -c my_twin_config.json
    ```
