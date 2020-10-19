<script>
  // create an empty person object
  let person = {
    firstName: "",
    lastName: "",
    age: ""
  };

  let people = [];

  function savePerson() {
    console.log("savePerson() clicked");
    db.collection("people")
      .doc(person.firstName)
      .set(person);
  }

  async function getPerson() {
    console.log("getPerson() clicked");
    let personDoc = await db
      .collection("people")
      .doc(person.firstName)
      .get();
    person = personDoc.data();
  }

  async function getPeople() {
    console.log("getPeople() clicked");
    let peopleCol = await db.collection("people").get();

    /*
    for (const personDoc in peopleCol) {
        people.push(personDoc.data())
    }*/

    peopleCol.forEach(personDoc => {
      people = [...people, personDoc.data()];
    });
  }
</script>

<!-- this is just a section to make the page look nice -->
<section class="content section">

  <h1>People</h1>

  <!-- a place to enter the first name -->
  <label>
    First name:
    <input bind:value={person.firstName} />
  </label>

  <!-- a place to enter the last name -->
  <label>
    Last name:
    <input bind:value={person.lastName} />
  </label>

  <!-- a place to enter the age -->
  <label>
    Age:
    <input bind:value={person.age} />
  </label>

  <button on:click={getPerson}>Get Person</button>

  <button on:click={savePerson}>Save Person</button>

  <button on:click={getPeople}>People</button>

  {#each people as person}
    {person.firstName}
    {person.lastName}
    {person.age}
     ,  
  {/each}

</section>
