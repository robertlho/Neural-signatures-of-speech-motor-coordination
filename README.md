### For my previous work on epilepsy, kindly refer to thesis excerpt titled "Wide-bandwidth electrographic characterisation of seizures in rat models of epilepsy using graphene micro-transistors"

The thesis discusses the value of incorporating infraslow frequency activity (< 0.1 Hz) in seizure onset zone localisation in rat models of chronic epilepsy (kainic acid and tetanus). Wide-bandwidth electrophysiological recordings were acquired by two-generations of graphene solution-gated field-effect transistors (gSGFETs).

# Neural-signatures-of-speech-motor-coordination

This repository records my journey to explore a potential shared neural basis between speech and speech-related gesturing. The AJILE12 dataset used contains long-term naturalistic human ECoG recordings with pose and behavioral classifications. 

## Motivation

The ability to flexibly modulate vocal pitch in humans plays a crucial role in conveying linguistic meaning through intonation patterns in speech. Recent research has shown that the dorsal laryngeal motor cortex (dLMC) is critically involved in controlling vocal pitch (Dichter et al., 2018).

![image](https://github.com/user-attachments/assets/8b81804d-5f2d-455c-a835-6b38e285b9c9)
##### ECoG electrodes visualised in MNI space (AJILE12 dataset).

Interestingly, there is growing evidence suggesting a shared neural basis for speech and motor expression, particularly in the context of gestures and their relationship to speech production and comprehension. Xu et al. (2009) demonstrated that symbolic gestures and spoken language activate overlapping brain regions, including the left inferior frontal gyrus (Broca's area) and the posterior superior temporal sulcus, which suggests that a common neural system processes both symbolic gestures and spoken language. 

It would be interesting to investigate the potential shared neural basis between vocal pitch modulation (a key aspect of speech prosody) and speech-related gestures. Gestures, such as hand and arm movements, often co-occur with speech and can convey additional linguistic and emotional information. These gestures are known to influence speech perception and comprehension (Biau & Soto-Faraco, 2013; Özyürek, 2014). If patterns are found between the dLMC and affective speech-related gestures, it may suggest that this region plays a role in integrating prosodic and gestural information during communication (Cacciante et al., 2024).

By exploring the spatial and temporal dynamics of the neural activity in the dLMC and other relevant brain regions, we can gain a deeper understanding of how prosodic and gestural information are integrated during the state of speech, and how this integration may support the rich expressive capabilities of human language.


## Data Processing and Exploratory Analysis
All code required to replicate this exploratory analysis can be found within ajile12_exploratory_analysis.ipynb.

#### Data Loading and Preprocessing

Load the NWB file containing the neural and behavioral data using the NWBHDF5IO class from the pynwb library.
Extract the coarse behavior labels and select the labels of interest (e.g., "Talk") and remove short duration events.

#### Electrode Selection and Filtering

Select a subset of electrodes of interest based on their MNI coordinates or other criteria (dLMC = 39, -21, 54 / -39, -21, 54).
Filter the neural data using a bandpass filter (e.g., Butterworth filter) to extract the desired frequency range (e.g., 70-150 Hz for high gamma).
Apply the Hilbert transform and convert the instantaneous amplitude to decibels (dB) to represent neural power.

#### Behavioral Event Segmentation

Identify the start and stop times of the behavioral events of interest (e.g., "Talk" events) from the coarse labels.
Segment the neural power data and pose data based on the start and stop times of the behavioral events.
Ensure that the segmented data chunks are of sufficient length for further analysis (e.g., longer than a minimum filter length).

#### Data Visualization

Plot the spectrogram of the neural power data for each selected electrode to visualize the power distribution across frequencies and time.

#### Data Alignment and Combination

Downsample the neural power data to match the sampling rate of the pose data.
Combine the downsampled neural power data and pose velocity data into a single dataframe.
Filter out instances with low velocity (e.g., < 200 pixels/sec) to focus on meaningful movements.

#### Correlation Analysis

Calculate the Pearson correlation coefficient between the neural power and pose velocity data.

#### Co-synchrony Analysis

Calculate suitable co-synchrony measures between the neural power time series of the dLMC and other motor cortex electrodes. Some common measures include:

Cross-correlation function between the neural power time series of the dLMC and motor cortex electrodes at different time lags. The peak cross-correlation value and corresponding time lag can indicate the strength and delay of synchronization between the two regions.

Coherence: Estimate the spectral coherence between the neural power time series of the dLMC and motor cortex electrodes. Coherence quantifies the degree of phase synchronization between the two regions as a function of frequency.

Phase-locking value (PLV): Calculate the PLV between the instantaneous phases of the neural power time series of the dLMC and motor cortex electrodes. PLV measures the consistency of phase differences between the two regions across time.



## Results and Interpretation

The correlation analysis revealed a statistically significant positive correlation (r = 0.36, p < 0.05) between neural power and pose velocity in one participant. This suggests that there is a moderate relationship between the high gamma power in the selected electrodes and the velocity of the right shoulder movement during "Talk" events.

However, it is important to note that correlation in one participant does not imply causation, and further analysis and experiments would be needed to establish any causal relationship between neural activity and movement. Additionally, the moderate correlation coefficient indicates that there may be other factors influencing the relationship between neural power and pose velocity.

Co-synchrony analysis supports the hypothesis of a functional coupling between the dLMC and motor cortex during speech production and gesture. The synchronization of neural activity between these regions may reflect the coordination of laryngeal and limb motor control, potentially facilitating the integration of speech and gesture in communication.

However, it is important to consider that the observed co-synchrony may not necessarily imply direct communication between the dLMC and motor cortex. The synchronization could arise from common input or modulatory influences from other brain regions involved in speech and gesture processing. Further investigation using techniques such as effective connectivity analysis or stimulation studies could help elucidate the directionality and causal relationships between these regions.

Future steps could include exploring further temporal dynamics of the neural-pose relationship, investigating other frequency bands or brain regions, and comparing the results across different behavioral events or participants. Incorporating additional data modalities, such as speech recordings or other movement parameters, could provide a more comprehensive understanding of the neural mechanisms underlying speech and gesture coordination.


## References
Dichter BK, Breshears JD, Leonard MK, Chang EF. The Control of Vocal Pitch in Human Laryngeal Motor Cortex. Cell. 2018 Jun 28;174(1):21-31.e9. doi: 10.1016/j.cell.2018.05.016. PMID: 29958109; PMCID: PMC6084806.

Xu J, Gannon PJ, Emmorey K, Smith JF, Braun AR. Symbolic gestures and spoken language are processed by a common neural system. Proc Natl Acad Sci U S A. 2009 Dec 8;106(49):20664-9. doi: 10.1073/pnas.0909197106. Epub 2009 Nov 18. PMID: 19923436; PMCID: PMC2779203.

Biau E, Soto-Faraco S. Beat gestures modulate auditory integration in speech perception. Brain Lang. 2013 Feb;124(2):143-52. doi: 10.1016/j.bandl.2012.10.008. Epub 2013 Jan 19. PMID: 23333667.

Özyürek A. Hearing and seeing meaning in speech and gesture: insights from brain and behaviour. Philos Trans R Soc Lond B Biol Sci. 2014 Sep 19;369(1651):20130296. doi: 10.1098/rstb.2013.0296. PMID: 25092664; PMCID: PMC4123675.

Cacciante L, Pregnolato G, Salvalaggio S, Federico S, Kiper P, Smania N, Turolla A. Language and gesture neural correlates: A meta-analysis of functional magnetic resonance imaging studies. Int J Lang Commun Disord. 2024 May-Jun;59(3):902-912. doi: 10.1111/1460-6984.12987. Epub 2023 Nov 16. PMID: 37971416.

## License

Link to the AJILE12 GitHub: https://github.com/BruntonUWBio/ajile12-nwb-data

All Rights Reserved
