## Flows

This folder contains the lists of flows detected, plus the ground truths.

### Flow Lists

The lists of flows are contained in files beginning with all_flows_. These flows are in a modified AQL-Answer format, to which we have added additional information as tags. Notably, the flow tag includes an ID, which is how we reference flows later.

### Ground Truths

Ground truths are included in a file called groundtruths.xml. These files contain a subset of flows from the corresponding all_flows_*.xml file, but with an additional element, "classification", which encodes the ground truth. The ground truth can be one of the following:

TRUE: a true positive
FALSE: a false positive
UNKNOWN: inconclusive because data flow was too complex.
NATIVE: inconclusive because the flow went into native code.
MISMATCH: inconclusive because the source and/or sink could not be found in the source code.


### Justifications

The justifications for the classifications can be found in the justifications folder. Each subfolder corresponds to a flow ID, and includes at least two different justifications as generated by our student workers.

We did not mandate a specific way to encode flows, as we thought that giving students the choice on how to do their investigation would strengthen the confidence of their classifications. Among the various formats chosen are graphml files (which can be opened with yEd or other apps), draw.io files (which can be opened with draw.io), or text descriptions.

Since many sources and sinks repeat themselves, we gave students an option to crossreference past classifications they performed for their justification. If a justification crossreferences another investigation, we include those in files beginning with 'crossref-'

Finally, at conflict meetings, we did not require the student with the incorrect justification to resubmit a new justification in order to save time; instead, this student only had to submit an empty file called "changed_per_conflict_meeting.txt". In these cases, one should look at the other justification as it has been reviewed and accepted by multiple people.

### Example

We consider the file [flows/justifications/ds-0/justification_0/graph.graphml](https://github.com/amordahl/fdroid_package/blob/ISSTA2021/flows/justifications/ds-0/justification_0/graph.graphml) as an example. Upon opening the graph in a graphml viewer (such as yEd), we see the following graph.


<img width="660" alt="Screen Shot 2021-04-29 at 10 03 31 PM" src="https://user-images.githubusercontent.com/9604243/116643465-bf6f4280-a936-11eb-9e67-7947b8224440.png">



This student performed a backward investigation from the sink, `sendBroadcast(requestScan)` as seen in [flows/all_flows_droidsafe.xml](https://github.com/amordahl/fdroid_package/blob/ISSTA2021/flows/all_flows_droidsafe.xml). They traced the origin of `requestScan` and found that it was defined on line 630, as a new Intent. The student searched for other useages of `requestScan`, and found that on line 632, data was added to `requestScan` through the `setData` method. The `file` parameter is a variable, and such, a possible source of taint. The student tracked the creation of the `file` variable and found it was created on line 596, from `path` -- again, the student tracked this variable to find that it was set as the device's screenshot folder on line 593. Since this is a constant, and does not contain data passed from a source, the student classified it as a false positive.
