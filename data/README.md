# Data directory

Raw datasets are not committed to this repository.

To reproduce the four BSDS-based sample experiments, run from the repository root:

```bash
python scripts/download_bsds_samples.py
```

The script downloads the official Berkeley `BSR_bsds500.tgz` archive and extracts only these IDs:

- `100007`
- `100039`
- `101027`
- `102062`

Expected local layout after setup:

```text
data/
└── bsds/
    ├── images/
    │   ├── 100007.jpg
    │   ├── 100039.jpg
    │   ├── 101027.jpg
    │   └── 102062.jpg
    └── gt/
        ├── 100007.mat
        ├── 100039.mat
        ├── 101027.mat
        └── 102062.mat
```

`data/bsds/` is ignored by Git and should stay local.

Official BSDS500 resource page:
https://www2.eecs.berkeley.edu/Research/Projects/CS/vision/grouping/resources.html
