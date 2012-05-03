#################
Overview of Romeu
#################

Romeu is designed to collect and display information about theater. As a result, its data model is
centered around these categories of data:

* **Creators**. A "creator" is a person or organization. This includes actors, directors,
  photographers, theater companies, awards organizations, and more.
* **Work Records**. A "work record" is a piece which may be performed. This includes play scripts,
  musical scripts and songbooks, choreographic documents, etc., as well as non-theatrical works such
  as novels which were later adapted into performable pieces. Work records are linked to creators
  (their authors) and to productions (their own performances).
* **Locations**. A "location" is a geographic area that can range from an entire country to a single
  street address. Location records are linked to a Country object and (optionally) a City object.
  Beyond these linkages, a location record contains a variety of fields for capturing address
  information or even latitude, longitude, and altitude measurements. A location may also be
  assigned a "venue type," indicating that productions (below) occur at this location.
* **Productions**. A "production" is a performance (or series of performances) occurring at a given
  location and with a particular set of cast members and other creators (directors, producers,
  etc.).
* **Festivals** and **Festival Occurrences**. A "festival" is a series of festival occurrences under
  a certain name. A "festival occurrence" is a collection of Productions, performed within a given
  set of locations and times, as part of a festival series. All festival occurrences, even if they only occur
  once, are assigned to a Festival.
* **Digital Objects**. A "digital object" is an uploaded file (or set of files) corresponding to
  a single physical object. Digital objects may be images, audio, or video recordings. In addition
  to the files themselves, each digital object record contains information about itself, such as its
  title and the date it was created, archival information such as which repository and collection it
  belongs to, and links to creators, work records, productions, locations, or festival occurrences,
  as needed.

Each of these models links with other models in a variety of ways, as well as linking with
subsidiary models that either provide a controlled vocabulary (such as the DigitalObjectType model) 
or add additional information to relationships (such as the CastMember model, which links creators
to productions in a particular way).
