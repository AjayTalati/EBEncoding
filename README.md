# EBEncoding - efficient bitwise encoding for temporal (medical) data
Episode Bitwise Encoding is an encoding method designed for abstracting multiple medication episodes of a patient related to a certain event, e.g., an adverse drug event. This being said, such encoding is actually domain independent and can be applied to any temporal events. 

> The typical purpose of such encoding is to make time series data easy to be consumed. One time series (e.g., a drug usage in past months) can be encoded as one numberic value. Multiple time series (e.g., multipharmacy) can be encoded as a vector. Therefore, after encoded, such data can be easily analysed (e.g., used in off-the-shelf machine learning algorithms).


## Example: Medication Episode Encoding for an Adverse Drug Event
<p>
  <img src="https://github.com/Honghan/EBEncoding/blob/master/EBEncoding/imgs/fig2.png" width="468"/>
</p>
*Figure 1. The bitwise encoding of dynamic medication episodes for a given adverse event*

Figure 1 illustrates an example encoding scenario. The upper part shows the sample medication episodes of a patient that are related to a certain inspecting time spot (marked as AE date in the figure). The encoding is realised through the following steps.

1. From the the inspecting time spot, look back to a certain period of time, e.g., 20 days. In our experiments, we selected 30 days. 
2. Split the time period into intervals using a unit of time, e.g., a day or a week. Number each interval in an ascendent order (started from zero) starting from the inspecting time spot. For example, 4 intervals are identified and numbered in `Figure 1`.
3. Add the patient's medication episodes to the timeline. 
4. For each medication episode, allocate a sequence of bits aligned with the interval order we obtained in step 2. The interval numbered zero is aligned with the most significant bit in the sequence. For each interval, set the corresponding bit to `1` if the interval intersects with the episode, and to `0` otherwise. For example, Medication Episode 2 (M2) in Figure~\ref{fig:coding} spans across interval number `1` and `2`. Its encoding is `0110` in binary code.
5. Repeat step 4 for all medication episodes. We will get a vector representing all the relevant medication episodes of the inspecting time spot.

## Usage
- The input data is simply a set of events, each of which has two attributes (`EpisodeStartDate`, `EpisodeEndDate`).
- The output data is a replica of the input except that a new attribute `encoded` is added to store the encoded value.

Using the script is quite simple. Get the python script in your working folder. And run it as follows.
```
python EBEncoding.py
```
The script will look for all files ended with `csv` extention as the input data (please refer the sample input `sample_input.csv` file for the format). Essentially, each row of the input file is an event with two attributes (`EpisodeStartDate`, `EpisodeEndDate`). The output file will be named by adding '_o' at the end of the input file, where each row will be with an additional column of the encoding result.

Sample input:
```
Drug,EpisodeStartDate,EpisodeEndDate,id,type,ADE_Date
Olanzapine,2010-12-14 00:00:00.000,2010-12-14 00:00:00.000,182,Enuresis,2010-12-14 00:00:00.000
```

Sample output:
```
Drug,EpisodeStartDate,EpisodeEndDate,id,type,ADE_Date,encoded
Olanzapine,2010-12-14 00:00:00.000,2010-12-14 00:00:00.000,182,Enuresis,2010-12-14 00:00:00.000,2147483648
```

##Questions?
This is my ongoing work (2016) at Kings College London. Any questions please email: honghan.wu@kcl.ac.uk.

##citation
Honghan Wu, Zina Ibrahim, Ehtesham Iqbal and Richard Dobson. Predicting Adverse Events from Multiple and Dynamic Medication Episodes - a preliminary result in a large mental health registry. Accepted by IJCAI 2016 - Workshop on Knowledge Discovery in Healthcare Data.