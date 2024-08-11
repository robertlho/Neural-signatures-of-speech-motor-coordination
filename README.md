### For my work on epilepsy, kindly refer to thesis excerpt titled "Wide-bandwidth electrographic characterisation of seizures in rat models of epilepsy using graphene micro-transistors"

The thesis discusses the value of incorporating infraslow frequency activity (< 0.1 Hz) in seizure onset zone localisation in rat models of chronic epilepsy (kainic acid and tetanus). Wide-bandwidth electrophysiological recordings were acquired by two-generations of graphene solution-gated field-effect transistors (gSGFETs).

# Neural-signatures-of-speech-motor-coordination

This repository records my journey to explore the shared neural basis between speech and speech-related gesturing. The AJILE12 dataset used contains long-term naturalistic human ECoG recordings with pose and behavioral classifications. 

## Motivation

The dorsal laryngeal motor cortex (dLMC), situated within the middle precentral gyrus (midPrCG), plays a crucial role in vocal pitch modulation for conveying linguistic meaning through intonation patterns in speech (Bouchard et al., 2013; Dichter et al., 2018). Growing evidence suggests a shared neural basis for speech and motor expression, particularly in gestures related to speech production and comprehension, with overlapping activations in left inferior frontal and posterior superior temporal regions (Xu et al., 2009).

![image](https://github.com/user-attachments/assets/8b81804d-5f2d-455c-a835-6b38e285b9c9)
##### Figure 1. ECoG electrodes visualised in MNI space (AJILE12 dataset).

Gestures often co-occur with speech, conveying additional linguistic and emotional information, and influencing speech perception and comprehension (Biau & Soto-Faraco, 2013; Özyürek, 2014). Co-speech gestures are thought to have both communicative and lexical retrieval facilitation functions, helping the speaker convey meaning and access words (Brady et al., 2016). Cortical areas involved in semantic processing and multimodal integration, such as the left inferior frontal gyrus (IFG) and middle temporal gyrus (MTG), have been implicated in processing both speech and gestures (Jouravlev et al., 2019; Straube et al., 2012).

If patterns are found between the dLMC and affective speech-related gestures, it may suggest this region's role in integrating prosodic and gestural information particularly during emotional communication, when pitch is elevated and gestures are exaggerated (Cacciante et al., 2024). This would be consistent with the view that co-speech gestures are tightly coupled with spoken language processing rather than being processed independently (Cacciante et al., 2024). Exploring the spatiotemporal dynamics of neural activity in the dLMC and other motor cortex regions can deepen our understanding of how prosodic and gestural information are coordinated during communication.

## Results and Interpretation

The correlation analysis between dLMC neural power and gesture velocity revealed a statistically significant positive correlation (r = 0.13, p < 0.001), supporting our hypothesis that affective gestures which co-occur with higher-pitched, i.e. more emotional affective vocalization. This moderate relationship between high gamma power in the selected electrodes and shoulder movement velocity during "Talk" events suggests other factors may also influence this relationship. Correlation in one participant certainly does not imply causation, and further analysis and experiments would be needed to establish any causal relationship between neural activity and movement. Additionally, the moderate correlation coefficient indicates that there may be other factors influencing the relationship between neural power and pose velocity.

![image](https://github.com/user-attachments/assets/a0cd2165-182c-4209-9f86-6aeb0e25b00f)
##### Figure 2. Extracted ECoG electrodes of the dLMC (blue) and wrist motor cortex (green). It is based on the neural power time series from these two sets of electrodes that spectral coherence is calculated.

Notably, the co-synchrony analysis yielded an average coherence of 0.3156 between the left dLMC and left wrist motor cortex electrodes, supporting the hypothesis of functional coupling between the dLMC and wrist motor cortex during speech production and gesture. Neurophysiologically, this synchronization may reflect coordinated oscillatory activity facilitating communication between these adjacent cortical areas. Articulatory and manual motor systems may be bound together, enabling co-speech gesture processing or integration.

However, this co-synchrony does not necessarily imply direct interactions between the dLMC and wrist motor cortex, as it could arise from common subcortical input or network-level synchronization orchestrated by other integration hubs (Cacciante et al., 2024). For example, the left IFG has been proposed as a key convergence zone for speech and gesture processing (Straube et al., 2012; Willems et al., 2007). High-resolution co-synchrony analysis and stimulation studies could help elucidate the network topology and causal interactions of the speech-gesture integration circuit.

The midPrCG, encompassing the dLMC, exhibits unique sensorimotor and multisensory responses relevant for speech (Silva et al., 2022). Its involvement in laryngeal motor control, auditory processing, and grapho-phonological associations aligns well with a role in phonological-motoric aspects of speech production. The current findings reinforce the midPrCG as an essential node for speech motor control and syllabic sequencing, augmenting the traditional focus on Broca's area (IFG).

Ongoing work aims to further characterize the temporal dynamics of the neural-gestural coupling and investigate its modulation across behavioral contexts and participants. Incorporating additional data modalities like speech recordings will enable temporally precise quantification of prosodic-gestural relationships. Linking neural, kinematic, and acoustic analyses could provide a more comprehensive picture of the spatiotemporal neural dynamics underlying speech and gesture coordination.


## Methods: Data Processing and Exploratory Analysis
All code required to replicate this exploratory analysis can be found within ajile12_dLMC_exploratory.ipynb.

#### Data Loading and Preprocessing

Load the NWB file containing the neural and behavioral data using the NWBHDF5IO class from the pynwb library.
Extract the coarse behavior labels and select the labels of interest (e.g., "Talk") and remove short duration events.

#### Electrode Selection and Filtering

Select a subset of electrodes of interest based on their MNI coordinates (dLMC = 39, -5, 40 / -39, -5, 40), (wrist motor cortex = 39, -8, 60 / -39, -8, 60).

Filter the neural data using a bandpass filter to extract the desired frequency range (70-150 Hz for high gamma).
Apply the Hilbert transform and convert the instantaneous amplitude to decibels (dB) to represent neural power.

![image](https://github.com/user-attachments/assets/d82e861b-84c7-48f1-8ed4-36131efe2eb5)
##### Figure 3. Visualizing the power distribution across frequencies and time, for each selected electrode. Representative illustration of six dLMC electrodes in the first subject.


#### Behavioral Event Segmentation

Identify the start and stop times of the behavioral events of interest (e.g., "Talk" events) from the coarse labels.
Segment the neural power data and pose data based on the start and stop times of the behavioral events.
Ensure that the segmented data chunks are of sufficient length for further analysis (e.g., longer than a minimum filter length).

#### Data Alignment and Combination

Downsample the neural power data to match the sampling rate of the pose data.
Combine the downsampled neural power data and pose velocity data into a single dataframe.
Filter out instances with low velocity (e.g., < 200 pixels/sec) to focus on meaningful movements.

#### Correlation Analysis

Calculate the Pearson correlation coefficient between the neural power and pose velocity data.

#### Co-synchrony Analysis

To investigate the functional coupling between the dLMC and wrist motor cortex during speech production and gesture, we estimated the spectral coherence between the neural power time series of the dLMC and wrist motor cortex electrodes. Coherence quantifies the degree of phase synchronization between the two regions as a function of frequency. It ranges from 0 (no coherence) to 1 (perfect coherence) and can reveal frequency-specific synchronization patterns. Significant coherence at specific frequencies may indicate coordinated oscillatory activity facilitating communication between the dLMC and wrist motor cortex.


## Conclusion
The moderate correlation and significant co-synchrony between dLMC and wrist motor cortex during speech production provides preliminary support for a shared neural substrate integrating prosodic and gestural information. The midPrCG likely plays a key role in coordinating vocal and manual motor systems to produce affective speech and congruent gestures. Further research linking neural, kinematic, and acoustic analyses is needed to elucidate the precise causal interactions and temporal dynamics of this speech-gesture integration network. These insights can inform the understanding of neural mechanisms of multimodal communication and guide development of neurorehabilitation approaches enhancing gesture use in aphasia.

## References

Bouchard KE, Mesgarani N, Johnson K, Chang EF. Functional organization of human sensorimotor cortex for speech articulation. Nature. 2013 Mar 21;495(7441):327-32. doi: 10.1038/nature11911. Epub 2013 Feb 20. Erratum in: Nature. 2013 Jun 27;498(7455):526. PMID: 23426266; PMCID: PMC3606666.

Dichter BK, Breshears JD, Leonard MK, Chang EF. The Control of Vocal Pitch in Human Laryngeal Motor Cortex. Cell. 2018 Jun 28;174(1):21-31.e9. doi: 10.1016/j.cell.2018.05.016. PMID: 29958109; PMCID: PMC6084806.

Xu J, Gannon PJ, Emmorey K, Smith JF, Braun AR. Symbolic gestures and spoken language are processed by a common neural system. Proc Natl Acad Sci U S A. 2009 Dec 8;106(49):20664-9. doi: 10.1073/pnas.0909197106. Epub 2009 Nov 18. PMID: 19923436; PMCID: PMC2779203.

Biau E, Soto-Faraco S. Beat gestures modulate auditory integration in speech perception. Brain Lang. 2013 Feb;124(2):143-52. doi: 10.1016/j.bandl.2012.10.008. Epub 2013 Jan 19. PMID: 23333667.

Özyürek A. Hearing and seeing meaning in speech and gesture: insights from brain and behaviour. Philos Trans R Soc Lond B Biol Sci. 2014 Sep 19;369(1651):20130296. doi: 10.1098/rstb.2013.0296. PMID: 25092664; PMCID: PMC4123675.

Brady MC, Kelly H, Godwin J, Enderby P, Campbell P. Speech and language therapy for aphasia following stroke. Cochrane Database Syst Rev. 2016 Jun 1;2016(6):CD000425. doi: 10.1002/14651858.CD000425.pub4. PMID: 27245310; PMCID: PMC8078645.

Cacciante L, Pregnolato G, Salvalaggio S, Federico S, Kiper P, Smania N, Turolla A. Language and gesture neural correlates: A meta-analysis of functional magnetic resonance imaging studies. Int J Lang Commun Disord. 2024 May-Jun;59(3):902-912. doi: 10.1111/1460-6984.12987. Epub 2023 Nov 16. PMID: 37971416.

Jouravlev O, Zheng D, Balewski Z, Le Arnz Pongos A, Levan Z, Goldin-Meadow S, Fedorenko E. Speech-accompanying gestures are not processed by the language-processing mechanisms. Neuropsychologia. 2019 Sep;132:107132. doi: 10.1016/j.neuropsychologia.2019.107132. Epub 2019 Jul 2. PMID: 31276684; PMCID: PMC6708375.

Straube B, Green A, Weis S, Kircher T. A supramodal neural network for speech and gesture semantics: an fMRI study. PLoS One. 2012;7(11):e51207. doi: 10.1371/journal.pone.0051207. Epub 2012 Nov 30. PMID: 23226488; PMCID: PMC3511386.

Willems RM, Ozyürek A, Hagoort P. When language meets action: the neural integration of gesture and speech. Cereb Cortex. 2007 Oct;17(10):2322-33. doi: 10.1093/cercor/bhl141. Epub 2006 Dec 11. PMID: 17159232.

Silva AB, Liu JR, Zhao L, Levy DF, Scott TL, Chang EF. A Neurosurgical Functional Dissection of the Middle Precentral Gyrus during Speech Production. J Neurosci. 2022 Nov 9;42(45):8416-8426. doi: 10.1523/JNEUROSCI.1614-22.2022. PMID: 36351829; PMCID: PMC9665919.

## License

Link to the AJILE12 GitHub: https://github.com/BruntonUWBio/ajile12-nwb-data

All Rights Reserved
