# Lab 7 - Looping and Conditional Rendering

## Intro

In this lab you will be seeing how we can display an array of data on the web page, and how to make things become part of the DOM or be excluded from the DOM.

## Pre-requisites

You should have a running "hello world" application with the song list component.

## 1. Render an array of objects

All the following steps should be undertaken in the song list component:

1. **Remove** the changeSong method + associated button

2. **Create a list** of song objects as an array, and **make this a stateful variable**. Ensure that each song is by a different artist.

3. **Remove** the previous stateful song variables

4. **Amend** the addvote method to include the ID of the song to vote for as a parameter and use this to update the correct song.

5. Create a variable which takes the existing array and converts it to an **array of well formed JSX elements**, each with a unique key (ignore the recordVote property for now)

6. **Bind the array** into the JSX returned from the components.

7. Check that the songs are shown on the web page (note that the vote buttons won't work.)

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const SongList = () => {

    const initialSongs : SongType[] = [
        {title : 'Last thing on my mind', artist: 'steps', votes : 0},
        {title : 'If you\'re over me', artist: 'Years and years', votes : 0},
        {title : 'Top of the world', artist: 'Carpenters', votes: 0},
        {title: 'Sometimes', artist: 'Erasure', votes : 0}
    ];

    const [songs, setSongs] = useState<SongType[]>(initialSongs);

    const addVote = (id : number) : void => {
        const newSongs = songs.map( (song,idx) => idx === id? {...song, votes: song.votes + 1} : song);
        setSongs(newSongs);
    }

    const displaySongs : JSX.Element[] = songs.map ( (song, index) => {
         return (<Song key={index} song={song} recordVote={() => addVote(index)} />);
    });

    return (
        <div>
            <h2>Your favourite songs are:</h2>
            <ul>{displaySongs}</ul>
        </div>
    );
};
```

## 2. Adjust the code so that the transformation from array to array of JSX objects is contained within the return statement

1. **Remove the variable** displaySongs, and instead bind the mapping to JSX elements within the return statement.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
return (
        <div>
            <h2>Your favourite songs are:</h2>
            <ul>
                songs.map((song, index) => <Song key={index} song={song} recordVote={() => addVote(index)}  />)
            </ul>
        </div>
    );
```


## 3. Use conditional rendering

Note: this is a particuarly challenging exercise!

All the following steps should be undertaken in the song list component:

1. **Create a stateful boolean variable** called "showAll", with an initial value of `true`.

2. **Create a button** and an associated function to toggle the value of showAll. 

    Make the text of the button change based on the value of showAll, **using the ternary operator**.

    If showAll is true, the button text should say "show only songs with 2 or more votes". If showAll is false, the button should say "show all songs".

3.  Display the relevant songs list within the component **using the logical && operator**.
 
    If showAll is true, show all the songs. 
    
    If it is false show only those songs that have 2 or more votes. 

    Hint: You may need to provide the binding to the songs list twice.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
import Song from './Song';
import { ReactElement, useState } from 'react';
import { SongType } from './SongTypes';

const SongList = () : ReactElement => {

    const initialSongs : SongType[] = [
        {title : 'Last thing on my mind', artist: 'steps', votes : 0},
        {title : 'If you\'re over me', artist: 'Years and years', votes : 0},
        {title : 'Top of the world', artist: 'Carpenters', votes: 0},
        {title: 'Sometimes', artist: 'Erasure', votes : 0}
    ];


    const [showAll, setShowAll] = useState<boolean>(true);
    const [songs, setSongs] = useState<SongType[]>(initialSongs);
    const [visible, setVisible] = useState<boolean>(false);

    const toggleVisibility = () : void => {
        setVisible(!visible);
    }

    const toggleShowAll = () : void => {
        setShowAll(!showAll);
    }

    const addVote = (id : number) : void => {
        const newSongs = songs.map( (song,idx) => idx === id? {...song, votes: song.votes + 1} : song);
        setSongs(newSongs);
    }

    return (
        <div>
            <h2>Your favourite songs are:</h2>
            <button onClick={toggleVisibility}> {visible ? 'hide' : 'show'} songs</button>
            <ul style= {{display : visible ? 'block' : 'none'}}>

                {showAll &&
                    songs.map((song, index) => <Song key={index} song={song} recordVote={() => addVote(index)}  />)
                }

                {!showAll &&
                    songs.filter(it => it.votes >=2).map((song, index) => <Song key={index} song={song} recordVote={() => addVote(index)}  />)
                }
            </ul>
            <h3>Currently showing {showAll ? 'all' : '2 or more rated'} songs</h3>
            <button onClick={toggleShowAll}>Show {showAll ? 'all songs' : 'only songs with 2 or more votes'}</button>
        </div>);
}

export default SongList;
```
   
