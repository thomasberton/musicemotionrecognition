# musicemotionrecognition

# Project Title

One Paragraph of project description goes here

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

```
Give examples
```

### Installing

A step by step series of examples that tell you how to get a development env running

Say what the step will be

```
Give the example
```

And repeat

```
until finished
```

End with an example of getting some data out of the system or using it for a little demo

## Running the tests

Explain how to run the automated tests for this system

### Break down into end to end tests

Explain what these tests test and why

```
Give an example
```


This Github provides multiple databases on which music emotion classification can be based

\section{Metadata, audio files and lyrics collection}

As said previously, it does not exist a dataset that gained general acceptance. However, some ones stood out from the others and are more often used than others. As instance, it is notably the case of the \textit{Million Song Database}, largely used for MIR tasks \cite{bertin}. For its well-known reputation and ease of use, this is one of the datasets that has been chosen for this thesis. 
\\

The \textit{Million Song Dataset} (abr.\ MSD) is provided by The Echo Nest\footnote{Music platform for developers, which was originally a research spin-off from the MIT Media Lab to understand the audio and textual content of recorded music}. It is defined as a ``freely-available collection of audio features and metadata for a million contemporary popular music tracks." \cite{bertin}. In other words, the dataset gathers metadata and some features analysis for one million songs. One may precise that it does not include any audio nor emotions. Due to the large set of this set (originally 280 GB) only a subset of it, i.e.\ 10,000 songs (1$\%$ of the global dataset), has been considered to work on it. This subset is also provided by The Echo Nest, and songs are said to be collected randomly from the original MSD. Table 4.1 shows some of the 45 fields associated with each track in the database.


\begin{table}[!h]
\centering
\begin{tabular}{|c|c|}
  \hline
  \textbf{Features} & \textbf{Value} \\
  \hline
  artist\_mbid &  db92a151-1ac2-438b-bc43-b82e149ddd50  \\
  \hline
  %The musicbrainz.org\footnote{MusicBrainz is an open music encyclopedia that collects music metadata and makes it available to the public.} tags &  4  \\
  %\hline
  artist$\_$name &  Rick Astley \\
  \hline
   title & Never Gonna Give You Up \\
  \hline
   danceability & 0.0 (which means not analyzed) \\
  \hline 
   loudness &  -7.75 \\
  \hline
  year & 1987 \\
  \hline
\end{tabular}

\caption{Example of some features for a track in the MSD}
\end{table}

By looking at the features which have been already computed in the MSD, one may ask the interest of having audio files if some of their characteristics have already been computed. The reader should remember that these features have been computed on the whole song and not on the instrumental nor isolated vocals. Downloading the audio files to recompute the different respectively for the vocals and the instrumental is thus necessary.
\\

After collecting information about 10,000 songs, one must obtain the corresponding audio files. To achieve this, a process of two steps have been followed. First the \textit{Spotify} API \cite{spotify} provides, given the title and the author of a song, a 30 seconds preview for a song given the name of the artist and the song. Only analyzing 30 seconds of a song for the vocals and instrumental parts is a very common practise in MER and has been proved to be a good practise \cite{li}. Experimental results show that gives better performance for detecting the sentiment of the song rather than the whole song \cite{su}. However, the preview is given in the form of an url, which is not directly downloadable and thus not analyzable. Therefore a second step is required : converting each url into a \textit{wav} file. This step has been achieved by an online converter \cite{onlineconver}.
\\ 
 
As said previously, the Million Song Dataset does not include emotion tags in the set itself. To tackle this problem, a common practise consists of linking the songs with another data source, \textit{Last.fm}, which collects the corresponding tags \cite{miller2, hu}. It is the official website that groups song tags and song similarities of the Million Song Dataset. These tags can be extracted from the online website via the \textit{Last.fm} API or by the corresponding offline database. In this thesis, the second option has been opted. The database contains two columns : one for the unique song identifier, and one for the corresponding tag(s). The fact the MSD also provides the unique id allows to connect the two databases. Since the tags are subject to some discussion, the section 4.1.3 is dedicated to give more details about them tags their related emotions.
\\

Regarding the lyrics, multiple data sources exist such as \textit{musiXmatch} dataset, which is the official lyrics collection of the Million Song dataset \cite{musicxmatch}. The lyrics are gathered in a bag-of-words format \cite{manning} : the lyrics are defined as a binary vector, where each element of this vector is set a 1 if it is present in a most used 5,000 words dictionary, 0 otherwise. Beyond the fact it is probably easier to use for multiple purposes, distributing lyrics under this format avoid copyright issues too \cite{musicxmatch}. This database has been used in the beginning of the thesis, but since it removed some freedom (e.g. taking into account stop words or not, only considering 5000 words)
it was finally dropped. Another database has been thus used : Lyricwiki.org \cite{lyricwiki}. It is an online database, which is well-known for its broad coverage and classical format \cite{mckay}. Song title, artist must be given to identify correctly the lyrics. 

\subsection{Isolating Instrumental And Vocals}
Once the audio files and lyrics have been collected, one must separate the vocal from the instrumental. This is a problem known as \textit{source separation} problem \cite{chanrungutai}. Unfortunately easy-to-use algorithms don't exist for a such task and quality of the results varies \cite{berrian}.
\\

Hopefully there exists a well-known software, used both by NLP scientists and music producers, called RX7\footnote{manufactured by iZotope} \cite{rx7, berrian}. It provides a powerful interface for audio experiments and it often refers as an excellent software for noise removal \cite{sbatella}. Using this software in the context of Music Information Retrieval is unprecedented. This software has chosen because it provides a \textit{Music Rebalance} option, which is particularly useful for this source separation problem. Once the original audio file is loaded in the software, this option proposes multiple modifications of the audio as shown in Figure 4.1 : tweaking the loudness of the voice, bass and so on. In our case, two setups were important : either isolating the voice, either remove it. This infers the following adjustments shown at (a) and (b). The \textit{Gain} option is used to adjust the level (dB) of the voice/bass/percussion or the others. The \textit{sensivity} rate defines the percentage of the input signal considered as e.g.\ Voice by the separation algorithm. Set at low values, this induces the separation algorithm to narrowly define the vocal content of the signal. If the resulting signal contains less audio bleed from the other instruments, this reduces the vocal clarity due to its sharpened approach. Higher values help to reduce this lack of clarity but consider more bleeding from the other elements. For this work, \textit{sensivity} has been set to its default value : 0,5.



% [Part about the algorithm behind. Need more info because nothing is available on the internet, probably for copyright secret. Work in progress]

\begin{figure}[!h]
     \centering
     \subfloat[][Parameters to isolate the acapella]
     {\includegraphics[scale=0.5]{iso_voice.JPG}\label{<figure1>}}
     \hspace{1 cm}
     \subfloat[][Parameters to remove the vocals]
     {\includegraphics[scale=0.5]{iso_instru.JPG}\label{<figure2>}}
     \label{steady_state}
     \caption{The two different operations during the source separation process}
\end{figure}

% The process of isolating vocalss
% Regarding the previous works on this topic, most of them focused on isolating vocals from a stereo recording, where the position of the vocals  is useful for separation. However,


\section{Identifying Mood Categories}
As said previously, the perception of emotion is subjective. This means one song may be considered as \textit{calm} for a person, while another person would judge this song as \textit{happy}. This, once again, proves the challenging difficulties MIR must deal with. This arises the question of “how to correctly label a song ?". This is only more complicated by the fact that perceived emotions is influenced by the mood of the listener and the place he listens to the music \cite{huron}. For this reason, platforms such as \textit{All Music Guide} have invested a lot of time, money and human resources to annotate their music databases with high-quality emotions tags \cite{allmusic, kim}. For this reason they are unfortunately unlikely to share their data with the MIR research community. 
\\

Hopefully, there exists fast and free approaches to collect emotion annotations from human listeners. One may cite \textit{Last.fm}, which has already been mentioned above. This website allows users to associate social tags to songs through their audio player interface. In 2007, no more than different 960,000 tags were identified and used for annotate songs \cite{miller}. This is this dataset that has been used to determine the emotion of the songs in the database.
\\

Still, as the reader can expect, this dataset of tags is not deprived from weaknesses. As instance, it does not always include direct useful or desired emotion-related tags, such as \textit{sadness} or \textit{happiness}. It provides a range of different tags, which may be not relevant in this context of sentiment classification. Examples of irrelevant tags were related to a non-affective aspect (``beat", ``trance") or were matter of personal taste (e.g.\ ``bad", ``good"). Others tags may relate the style of the songs or the period of release (e.g.\ ``80s"), which are not useful in this context neither. Besides the fact that social tags contain irrelevant information, there exist ambiguous tags that can cause confusion. This is notably the case of the ``love" tag. Does it mean the song is about love or did the annotator mean he loved the song ? 
\\

Table 4.2 shows the most 10 frequent tags that can be found in the dataset. It confirms the idea that social tags can be noisy and not directly emotion-related. The fact the ``love" term is ranked 5$^{th}$ in the top 10 most frequent tags can give some clues about its ambiguous meaning too. 
\\

This shows the need of some cleanup. For this reason junk tags and tags with little or no affective meanings have been filtered out.\ A linguistic resource, \textit{WordNet-Affect}, has been particularly helpful for this task \cite{strapparava}. It is an extension of WordNet defined as ``a large lexical database of English nouns, verbs, adjectives and adverbs grouped into sets of synonyms i.e.\ synsets" \cite{miller3,wordnet}. Each synset represent a distinct concept". WordNet-Affect, its extension, provides a powerful support by linking each non-noun or noun tag to the emotion synset it refer. It helped to filter tags which bear an emotional meaning from the non-interesting tags. For instance \textit{WordNet-Affect} would output, given the tag ``sadly", the synset ``sad", whereas it would output nothing for a ``80s" tag.

%% Il faut clarifier les chiffres : combien de tags avec last fm et combien de mots uniques avec wordnet-affect. COmbien matchent ? 

\begin{table}[!h]
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


The junk tags being filtered out, the useful tags had been  converted into emotional tags. However, by looking closer at some of them, it existed some emotional tags that didn't represent distinguishable meanings. In fact, many of them were synonyms and needed to be grouped together. Hence the 150 remaining emotional tags belonging to and being derived from the same synset in WordNet-Affect were grouped together. Due to its tree-based structure, Wordnet-Affect can easily find the ``upper" emotional concept of an emotion. The reader can find an illustration of this tree in annex, at Figure A.3. As result the tags were merged into 19 groups, as shown in Table 4.3.
\\

For the classification experiments, each category should have enough samples to build reliable model. This is why categories with fewer than 20 songs were dropped, resulting in a database labeled with 15 emotional tags categories. The categories have been dropped are highlighted in blue in the Table 4.3.





% \section{Selecting the Songs}

% In a binary classification, each category needs negative samples as well. To create our negative sample set for a given category, an approach consist of chosing songs that are tagged with any of the terms found withing that category but heavily tagged with many other terms. Since a lot of negative samples could fit for each category, songs tagged with at least 15 others terms including mood terms in other categories were selected. 
 
% total = 2276


\begin{table}[!h]
\centering
\begin{tabular}{|c|c|}%c|}
    \hline
    \textbf{n$^{\circ}$} & \textbf{Categories} \\  %\textbf{\# of songs} \\
    \hline
    1 & calm, comfort, quiet, serene, mellow, chill out, chill \\ % & 170 \\
    \hline
    2 & sad, sadness, unhappy, melancholic, melancholy, misery,... \\ % & 187 \\
    \hline
    3 & happy, happiness, happy songs, happy music,... \\ % & 118  \\
    \hline
    4 & romantic, romantic music, affection, tender, love,...\\ %  & 788 \\
    \hline
    5 & upbeat, gleeful, high spirits, zest, enthusiastic,...\\ %  & 220 \\
    \hline
    6 & depressed, blue,dark, depressive, dreary,...\\ %  & 50 \\
    \hline
    7 & anger, angry, choleric, fury, outraged, rage, hostility, jealousy,..\\ %  & 49 \\
    \hline
    8 & grief, heartbreak, mournful, sorrow, sorry,... \\ % & 63 \\
    \hline
    9 & dreamy, captivation, admiration, awe, worship,... \\ % & 40 \\
    \hline
    10 & cheerful, cheer up, festive, jolly, jovial, merry,...\\ %  & 45 \\
    \hline
     \cellcolor{blue!25} 11 & \cellcolor{blue!25} brooding, contemplative, meditative,... \\ % & 22 \\
    \hline
    12 & aggression, aggressive, belligerence, bang,...\\ %  & 35 \\
    \hline
    13 & confident, encouraging, encouragement, optimism, confidence,... \\ % & 26 \\
    \hline
    14 & angst, anxiety, anxious, jumpy, nervous,...\\ %  & 202 \\
    \hline
    \cellcolor{blue!25} 15 &  \cellcolor{blue!25} earnest, heartfelt, earnestness,...\\ %  & 2 \\
    \hline 
     \cellcolor{blue!25}16 &  \cellcolor{blue!25}desire, hope, hopeful \\ % & 0 \\
    \hline
     \cellcolor{blue!25}17 &  \cellcolor{blue!25} pessimism, cynical, pessimistic, weltschmerz,... \\ % & 0 \\
    \hline
    18 & excitement, exciting, exhilarating, thrill, ardor,... \\ % & 137 \\
    \hline
    19 & shamefacedness, confusion, shame,...\\ %  & 122 \\
 
    \hline
\end{tabular}
\caption{The 19 emotional categories resulting from the Last.fm analysis}
\end{table}

\clearpage
