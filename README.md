
# Music Emotion Recognition and Classification Database

 One major drawback of Music Emotion Recognition is the lack of of cooperation across research institutes.  Each one uses a different database and conclusions from previous works are unfortunately not consistent. As result, these conclusions cannot be extended to other datasets. 

To address this problem, MIR researchers should establish benchmark emotional databases so that results would come from the same database and comparable.  A common database, analyzed by experts, would be time-saving and reduce errors such as songs labeled with a non-corresponding emotion.  To tackle this problem and contribute to the Music Emotion Recognition, we freely re- leased all the databases that have been used in this thesis.  The reader, and any enthusiastic person, will find here multiple databases on which music emotion classification can be based.


 ## Metadata, audio files and lyrics collection

As said previously, it does not exist a dataset that gained general acceptance. However, some ones stood out from the others and are more often used than others. As instance, it is notably the case of the \textit{Million Song Database}, largely used for MIR tasks. For its well-known reputation and ease of use, this is one of the datasets that has been chosen for this thesis. 

The Million Song Dataset (abr. MSD) is provided by The Echo Nest\footnote{Music platform for developers, which was originally a research spin-off from the MIT Media Lab to understand the audio and textual content of recorded music. It is defined as a "freely-available collection of audio features and metadata for a million contemporary popular music tracks." In other words, the dataset gathers metadata and some features analysis for one million songs. One may precise that it does not include any audio nor emotions. Due to the large set of this set (originally 280 GB) only a subset of it, i.e. 10,000 songs (1% of the global dataset), has been considered to work on it. This subset is also provided by The Echo Nest, and songs are said to be collected randomly from the original MSD. 


By looking at the features which have been already computed in the MSD, one may ask the interest of having audio files if some of their characteristics have already been computed. The reader should remember that these features have been computed on the whole song and not on the instrumental nor isolated vocals. Downloading the audio files to recompute the different respectively for the vocals and the instrumental is thus necessary.


After collecting information about 10,000 songs, one must obtain the corresponding audio files. To achieve this, a process of two steps have been followed. First the Spotify API provides, given the title and the author of a song, a 30 seconds preview for a song given the name of the artist and the song. Only analyzing 30 seconds of a song for the vocals and instrumental parts is a very common practise in MER and has been proved to be a good practise. Experimental results show that gives better performance for detecting the sentiment of the song rather than the whole song. However, the preview is given in the form of an url, which is not directly downloadable and thus not analyzable. Therefore a second step is required : converting each url into a \textit{wav} file. This step has been achieved by an online converter.

As said previously, the Million Song Dataset does not include emotion tags in the set itself. To tackle this problem, a common practise consists of linking the songs with another data source, \textit{Last.fm}, which collects the corresponding tags. It is the official website that groups song tags and song similarities of the Million Song Dataset. These tags can be extracted from the online website via the Last.fm API or by the corresponding offline database. In this thesis, the second option has been opted. The database contains two columns : one for the unique song identifier, and one for the corresponding tag(s). The fact the MSD also provides the unique id allows to connect the two databases.Since the tags are subject to some discussion, the section 4.1.3 is dedicated to give more details about them tags their related emotions.


Regarding the lyrics, multiple data sources exist such as musiXmatch dataset, which is the official lyrics collection of the Million Song dataset. The lyrics are gathered in a bag-of-words format : the lyrics are defined as a binary vector, where each element of this vector is set a 1 if it is present in a most used 5,000 words dictionary, 0 otherwise. Beyond the fact it is probably easier to use for multiple purposes, distributing lyrics under this format avoid copyright issues too. This database has been used in the beginning of the thesis, but since it removed some freedom (e.g. taking into account stop words or not, only considering 5000 words)
it was finally dropped. Another database has been thus used : Lyricwiki.org . It is an online database, which is well-known for its broad coverage and classical format. Song title, artist must be given to identify correctly the lyrics. 

## Isolating Instrumental And Vocals
Once the audio files and lyrics have been collected, one must separate the vocal from the instrumental. This is a problem known as source separation problem. Unfortunately easy-to-use algorithms don't exist for a such task and quality of the results varies .


Hopefully there exists a well-known software, used both by NLP scientists and music producers, called RX7 (manufactured by iZotope) . It provides a powerful interface for audio experiments and it often refers as an excellent software for noise removal . Using this software in the context of Music Information Retrieval is unprecedented. This software has chosen because it provides a \textit{Music Rebalance} option, which is particularly useful for this source separation problem. Once the original audio file is loaded in the software, this option proposes multiple modifications of the audio as shown in Figure 4.1 : tweaking the loudness of the voice, bass and so on. In our case, two setups were important : either isolating the voice, either remove it. This infers the following adjustments shown at (a) and (b). The Gain option is used to adjust the level (dB) of the voice/bass/percussion or the others. The \textit{sensivity} rate defines the percentage of the input signal considered as e.g. Voice by the separation algorithm. Set at low values, this induces the separation algorithm to narrowly define the vocal content of the signal. If the resulting signal contains less audio bleed from the other instruments, this reduces the vocal clarity due to its sharpened approach. Higher values help to reduce this lack of clarity but consider more bleeding from the other elements. For this work, sensivity has been set to its default value : 0,5.


## Identifying Mood Categories
As said previously, the perception of emotion is subjective. This means one song may be considered as calm for a person, while another person would judge this song as happy. This, once again, proves the challenging difficulties MIR must deal with. This arises the question of “how to correctly label a song ?". This is only more complicated by the fact that perceived emotions is influenced by the mood of the listener and the place he listens to the music \cite{huron}. For this reason, platforms such as All Music Guide have invested a lot of time, money and human resources to annotate their music databases with high-quality emotions tags. For this reason they are unfortunately unlikely to share their data with the MIR research community. 


Hopefully, there exists fast and free approaches to collect emotion annotations from human listeners. One may cite Last.fm, which has already been mentioned above. This website allows users to associate social tags to songs through their audio player interface. In 2007, no more than different 960,000 tags were identified and used for annotate songs. This is this dataset that has been used to determine the emotion of the songs in the database.


Still, as the reader can expect, this dataset of tags is not deprived from weaknesses. As instance, it does not always include direct useful or desired emotion-related tags, such as \textit{sadness} or \textit{happiness}. It provides a range of different tags, which may be not relevant in this context of sentiment classification. Examples of irrelevant tags were related to a non-affective aspect ("beat", "trance") or were matter of personal taste (e.g.\ "bad", "good"). Others tags may relate the style of the songs or the period of release (e.g.\ "80s"), which are not useful in this context neither. Besides the fact that social tags contain irrelevant information, there exist ambiguous tags that can cause confusion. This is notably the case of the "love" tag. Does it mean the song is about love or did the annotator mean he loved the song ? 


Table 4.2 shows the most 10 frequent tags that can be found in the dataset. It confirms the idea that social tags can be noisy and not directly emotion-related. The fact the "love" term is ranked 5th in the top 10 most frequent tags can give some clues about its ambiguous meaning too. 


<p align="center">
  <img width="460" height="300" src="https://github.com/thomasberton/musicemotionrecognition/blob/master/pictures/junktags.png">
</p>


This shows the need of some cleanup. For this reason junk tags and tags with little or no affective meanings have been filtered out. A linguistic resource, WordNet-Affect, has been particularly helpful for this task. It is an extension of WordNet defined as "a large lexical database of English nouns, verbs, adjectives and adverbs grouped into sets of synonyms i.e. synsets" . Each synset represent a distinct concept". WordNet-Affect, its extension, provides a powerful support by linking each non-noun or noun tag to the emotion synset it refer. It helped to filter tags which bear an emotional meaning from the non-interesting tags. For instanceWor dNet-Affect would output, given the tag "sadly", the synset "sad", whereas it would output nothing for a "80s" tag.

<!--%% Il faut clarifier les chiffres : combien de tags avec last fm et combien de mots uniques avec wordnet-affect. COmbien matchent ? 

<!--\begin{table}[!h]
\centering
\begin{tabular}{|c|c|}
    \hline
    \textbf{Tag} & \textbf{Total count} \\
    \hline
    rock & 101,071 \\
    \hline
    pop & 69,159 \\
    \hline
    alternative & 55,777 \\
    \hline
    indie & 48;175 \\
    \hline
    electronic & 48,175 \\
    \hline
    female vocalist & 42,565 \\
    \hline
    favorites & 39,921 \\
    \hline
    Love & 34,901 \\
    \hline
    dance & 33,618 \\
    \hline
    00s & 31,432 \\
    \hline
\end{tabular}
\caption{Top 10 most frequent tags in the \textit{Last.fm} dataset}
\end{table}






