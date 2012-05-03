################
Romeu data model
################

What follows is a full list of all models used in Romeu, along with a brief description and a list
of all linkages to other models.

***********************
Controlled vocabularies
***********************

These models serve only to provide a controlled but expandable vocabulary for use by other models.

* **WorkGenre** / **WorkCulture** / **WorkStyle**: Used by ``WorkRecord`` models. A work record's
  "Genre" represents the overall type of the performance (drama, comedy, etc.), "Culture" represents
  the culture a work is part of (Cuban, African American, etc.), and "Style" represents the period
  or movement that a work belongs to (Constructivism, New Wave, etc.).
* **Repository** / **Collection**: These fields are used by digital objects to align them with the
  University of Miami's ContentDM digital asset management system. Each repository contains a number
  of collections, and each collection contains a group of related digital objects.
* **License**: Provides a controlled vocabulary of digital object license types (copyright, Creative
  Commons, etc.) for use by digital objects.
* **DigitalObjectType**: Controlled vocabulary of digital object types. By default, includes "Video
  recording", "Audio recording", "Image", and "Other".
* **Country**: 
* **City**:
* **Language**:
* **WorkRecordType**:
* **WorkRecordFunction**:
* **DirectingTeamFunction** / **CastMemberFunction** / **DesignTeamFunction** / **TechTeamFunction**
  / **ProductionTeamFunction** / **DocumentationTeamFunction** / **AdvisoryTeamFunction**:
* **OrgFunction**:
* **FestivalFunction**:
* **PhysicalObjectType**:
* **VenueType**:

*******************
Primary data models
*******************

These models represent the main types of content captured by Romeu, which link to each other and to
the controlled vocabularies above.

* **Creator**

  The Creator model captures "people" of all kinds, from individuals to organizations. Creators have
  the following direct relationships with other models (relationships which are stored as part of
  the Creator model itself):

  | *birth_location* (Location)
  | *death_location* (Location)
  | *nationality* (Country)
  | *location* (Location; represents office/headquarters of an organization)
  | *related_creators* (Creator; uses an intermediary model, **RelatedCreator**, which stores metadata about the relationship)
  | *photo* (DigitalObject; a headshot or representative photo of the Creator)

* **Location**

  Stores locations of all kinds, from street addresses to entire countries.
