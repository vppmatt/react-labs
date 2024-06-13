# Lab 6 - Communicating between components

## Intro

In this lab you will be seeing how we can share stateful data between 2 components in a parent-child relationship.

## Pre-requisites

You should have a running "hello world" application with the song list component.

## 1. Pass a stateful variable to a child component

1. In the song list component, **create 2 local stateful javascript object variables** called song1 and song2, each containing a title and artist name of a song.

2. Change the JSX in the song list component so that it **passes a single property**, a song, to the child component.

3. **Amend the song component** so that the information is still correctly displayed on the web page.

4. **Create a button** in the SongList component that will change the name of each song to a different one by the same artist.

5. **Write a function** to be executed when the button is clicked that changes both songs. (Hint: use the setter methods and the spread operator). Bind the function to the button's onClick event.

6. Check that when the button is clicked, the children are re-rendered

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const SongList = () => {
    
    let [song1, setSong1] = useState({title : 'Last thing on my mind', artist: 'steps'});
    let [song2, setSong2] = useState({title : 'If you\'re over me', artist: 'Years and years'});

    const changeSong = () => {
        setSong1({...song1, title : 'Something in your eyes'});
        setSong2({...song2, title : 'King' })
    }

    return (
        <div>
            <ul>
                <Song song={song1} />
                <Song song={song2} />
            </ul>
            <button onClick={changeSong}>Change songs</button>
        </div>
    );
};
```

```
const Song = (props) => {
   return (<li>{props.song.title} by {props.song.artist}</li>);
}
```


## 2. Update a parent's stateful variable from a child component

In this part of the lab we are going to be creating a facility to give each song a number of votes, and then we can vote separately for each song.

1. With the SongList component, **adjust the 2 stateful song variables** to include an extra field - the number of votes for a song.

2. Within the Songlist component, **create 2 regular Javascript functions** to increment the number of votes for each song (call these voteForSong1, voteForSong2)

3. Within the Songlist component, within the JSX that defines each instance of the Song component, **add in an additional property** called recordVote that points to the relevant voting function.

4. In the song object **display the number of votes** the song has received within the JSX.

5. In the song object, **add a button** for each song, which when pressed will increment the number of votes for that song

6. In the song object, bind the voting button to the relevant voting function in the parent object.

7. Check that when the voting buttons are clicked, the number of votes for each song is correctly recorded.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.


```
import Song from './Song';
import {useState} from "react";

const SongList = () => {

    let [song1, setSong1] = useState({title : 'Last thing on my mind', artist: 'steps', votes : 0});
    let [song2, setSong2] = useState({title : 'If you\'re over me', artist: 'Years and years', votes : 0});

    const addVote1 = () => {
        setSong1({...song1, votes : song1.votes + 1});
    }

    const addVote2 = () => {
        setSong2({...song2, votes : song2.votes + 1});
    }

    const changeSong = () => {
        setSong1({...song1, title : 'Something in your eyes'});
        setSong2({...song2, title : 'King' })
    }

    return (
        <div>
            <h2>Your favourite songs are:</h2>
            <ul>
                <Song song={song1} recordVote={addVote1}/>
                <Song song={song2} recordVote={addVote2}/>
            </ul>
            <button onClick={changeSong}>Change songs</button>
        </div>
    );
};

export default SongList
```

```
const Song = (props) => {

   const voteNow = () => {
      props.recordVote();
   }

   return (<li>{props.song.title} by {props.song.artist} has {props.song.votes} votes. <button onClick={voteNow}>Vote for this song</button>
   </li>);
}

export default Song
```