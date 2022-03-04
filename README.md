# sentinel-dettect

Create a [DeTT&CT](https://github.com/rabobank-cdc/DeTTECT) techniques administration file from exported Microsoft Sentinel analytics rules. You'll still need to handle scoring and other customizations in the DeTT&CT editor, but hopefully this helps speed the process up!

This will generate a DeTT&CT [techinque object](https://github.com/rabobank-cdc/DeTTECT/wiki/YAML-administration-techniques_v1_2#technique-object) for each analytic rule, based on the assigned MITRE ATT&CK technique. The analytic rule name will be placed in the location field of the associated [detection object](https://github.com/rabobank-cdc/DeTTECT/wiki/YAML-administration-techniques_v1_2#detection-object). 

This currently maps against MITRE ATT&CK v10.1 techniques.

# Prerequisites

## Python requirements

Only requires the PyYAML library. Install with `pip3 install -r requirements.txt`.

## Sentinel requirements

Make sure that your analytics rules are updated with matching MITRE ATT&CK techniques. This script will ignore rules that don't have any assigned techniques.

# How to use

## Export rules
First, export your Sentinel analytics rules. You'll want to use the web interface to do this, as the Sentinel REST API and Azure CLI currently do not export techniques.

## Run the script

First, clone the repository `git clone https://github.com/h0ffayyy/sentinel-dettect.git`

Run the script from within the cloned directory

## Examples

Select an input file and output to default folder:

`python sentinel_dettect.py -f Azure_Sentinel_analytics_rules.json`

Select a directory containing files you want to convert:

`python sentinel_dettect.py -d input`

Customize the output location:

`python sentinel_dettect.py -f Azure_Sentinel_analytics_rules.json -o dettect_rules`

You'll find the generated techniques administration file `techniques_administration_sentinel.yml` in either the default `outputs` directory, or the one you specified with `-o`.

The technique object for the rule "Azure DevOps Audit Stream Disabled" with the tehcnique T1564 "Hide Artifacts" will look like the following:

```
techniques:
- technique_id: T1564
  technique_name: Hide Artifacts
  detection:
  - applicable_to:
    - all
    location:
    - microsoft-sentinel
    - Azure DevOps Audit Stream Disabled
    comment: ''
    score_logbook:
    - date: ''
      score: -1
      comment: ''
  visibility:
  - applicable_to:
    - all
    comment: ''
    score_logbook:
    - date: ''
      score: 0
      auto_generated: true
```


## Help output

```
$ python sentinel_dettect.py --help

usage: sentinel_dettect.py [-h] [-f FILE] [-d DIRECTORY] [-o OUTPUT]

Create a DeTT&CT techniques administration file from Microsoft Sentinel analytics rules

optional arguments:
  -h, --help            show this help message and exit
  -f FILE, --file FILE  the source file to convert to YAML
  -d DIRECTORY, --directory DIRECTORY
                        a source directory containing Sentinel rule files to convert
  -o OUTPUT, --output OUTPUT
                        specify a custom output file
```