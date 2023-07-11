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

4. **Remove** the addvote methods for now - we'll replace these later.

5. Create a variable which takes the existing array and converts it to an **array of well formed JSX elements**, each with a unique key (ignore the recordVote property for now)

6. **Bind the array** into the JSX returned from the components.

7. Check that the songs are shown on the web page (note that the vote buttons won't work.)

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

```
const SongList = () => {

    const initialSongs = [
        {title : 'Last thing on my mind', artist: 'steps', votes : 0},
        {title : 'If you\'re over me', artist: 'Years and years', votes : 0},
        {title : 'Top of the world', artist: 'Carpenters', votes: 0},
        {title: 'Sometimes', artist: 'Erasure', votes : 0}
    ];

    let [songs, setSongs] = useState(initialSongs);

    const displaySongs = songs.map ( (song, index) => {
         return (<Song key={index} song={song} />);
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
                {songs.map((song, index) => <Song key={index} song={song}/>)}
            </ul>
        </div>
    );
```

## 3. Re-implement the voting function

1. In the songlist component **create a new addVote method**. This method should take the artist as a parameter, and use functional syntax to convert the existing songs list into a new one with an updated vote value for the relevant entry.

    Hint: You cannot edit the array directly, so you need to create a new array and then use the setter method of the stateful variable.

    There are a few ways to code this - a sample solution is provided below:

2. In the songlist component **pass the addVote method** to the Song component using its' recordVote property.

3. In the Song Component, **call the parent addVote method**, passing through the position in the array. Hint: You do not want the method to be executed when the component is rendered so take care in how you impelement this.

### End of section code
This is a sample solution to the requirements - you may have other code present from previous exercises.

**SongList.js**
```
const SongList = () => {

    const initialSongs = [
        {title : 'Last thing on my mind', artist: 'steps', votes : 0},
        {title : 'If you\'re over me', artist: 'Years and years', votes : 0},
        {title : 'Top of the world', artist: 'Carpenters', votes: 0},
        {title: 'Sometimes', artist: 'Erasure', votes : 0}
    ];

    let [songs, setSongs] = useState(initialSongs);

    const addVote = (id) => {
        const newSongs = songs.map( (song,idx) => idx == id? {...song, votes: song.votes + 1} : song);
        setSongs(newSongs);
    }

    return (
        <div>
            <h2>Your favourite songs are:</h2>
            <ul>
               {songs.map((song, index) => 
                    <Song key={index} song={song} recordVote={addVote} id={index} />)
               }
            </ul>
        </div>
    );
};
```

**Song.js**
```
const Song = (props) => {

    const voteNow = () => {
       props.recordVote(props.id);
    }
 
    return (<li>{props.song.title} by {props.song.artist} has {props.song.votes} votes. <button onClick={voteNow}>Vote for this song</button>
    </li>);
 }
```

## 4. Use conditional rendering

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
const SongList = () => {

    const initialSongs = [
        {title : 'Last thing on my mind', artist: 'steps', votes : 0},
        {title : 'If you\'re over me', artist: 'Years and years', votes : 0},
        {title : 'Top of the world', artist: 'Carpenters', votes: 0},
        {title: 'Sometimes', artist: 'Erasure', votes : 0}
    ];

    let [showAll, setShowAll] = useState(true);
    let [songs, setSongs] = useState(initialSongs);

    const addVote = (artist) => {
        let newSongs = songs.map( it => {
            if (it.artist === artist) {
                return {...it, votes: it.votes + 1};
            }
            else {
                return it;
            }
        });
        setSongs(newSongs);
    }

    const toggleShowAll = () => {
        setShowAll(!showAll);
    }

    return (
        <div>
            <h2>Your favourite songs are:</h2>
            <ul>
                {showAll &&
                    songs.map((song, index) => <Song key={index} song={song} recordVote={addVote}/>)
                }

                {!showAll &&
                    songs.filter(it => it.votes >=2).map((song, index) => <Song key={index} song={song} recordVote={addVote}/>)
                }

            </ul>
            <h3>Currently showing {showAll ? 'all' : '2 or more rated'} songs</h3>
            <button onClick={toggleShowAll}>Show {showAll ? 'all songs' : 'only songs with 2 or more votes'}</button>
        </div>
    );
};
```
   
