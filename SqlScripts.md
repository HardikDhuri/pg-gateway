## Create a sql function to fetch the user

```sql
CREATE OR REPLACE FUNCTION user_search(uname TEXT) RETURNS TABLE (usename name, passwd text) as
$$
  WITH myuser as (SELECT $1 as fullusername) SELECT fullusername as usename, passwd FROM pg_catalog.pg_shadow INNER JOIN myuser ON usename = substring(fullusername from '[^@]*')
$$
LANGUAGE sql SECURITY DEFINER;


CREATE ROLE pgbuser WITH LOGIN PASSWORD 'pgbouncer';
GRANT EXECUTE ON FUNCTION user_search(text) TO pgbuser;
```

## Here are some seeding scripts:

```sql
CREATE TABLE character (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    race VARCHAR(50),
    power_level INT,
    home_planet VARCHAR(100),
    alignment VARCHAR(20)
);


INSERT INTO character (name, race, power_level, home_planet, alignment) VALUES
    ('Goku', 'Saiyan', 10000, 'Vegeta', 'Good'),
    ('Vegeta', 'Saiyan', 9000, 'Vegeta', 'Neutral'),
    ('Piccolo', 'Namekian', 5000, 'Namek', 'Good'),
    ('Frieza', 'Frieza Race', 1200000, 'Unknown', 'Evil'),
    ('Cell', 'Bio-Android', 1500000, 'Earth', 'Evil'),
    ('Gohan', 'Saiyan-Human Hybrid', 2000, 'Earth', 'Good'),
    ('Trunks', 'Saiyan-Human Hybrid', 4000, 'Earth', 'Good'),
    ('Krillin', 'Human', 2000, 'Earth', 'Good'),
    ('Bulma', 'Human', 5, 'Earth', 'Good'),
    ('Master Roshi', 'Human', 139, 'Earth', 'Good');


CREATE TABLE episode (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    season INT,
    episode_number INT,
    air_date DATE,
    arc VARCHAR(100),
    villain VARCHAR(100)
);

INSERT INTO episode (title, season, episode_number, air_date, arc, villain) VALUES
    ('The Arrival of Raditz', 1, 1, '1989-04-26', 'Saiyan Saga', 'Raditz'),
    ('The Worlds Strongest Team', 1, 3, '1989-05-17', 'Saiyan Saga', 'Raditz'),
    ('Vegeta Saga Begins', 1, 5, '1989-06-14', 'Saiyan Saga', 'Vegeta and Nappa'),
    ('The Battle with Vegeta', 1, 9, '1989-07-26', 'Saiyan Saga', 'Vegeta'),
    ('Frieza Saga Introduction', 2, 1, '1990-01-01', 'Frieza Saga', 'Frieza'),
    ('The Power of Goku', 2, 5, '1990-02-14', 'Frieza Saga', 'Frieza'),
    ('Cells Arrival', 3, 1, '1991-04-10', 'Cell Saga', 'Cell'),
    ('The Cell Games Begin', 3, 12, '1991-06-26', 'Cell Saga', 'Cell'),
    ('The Return of Majin Buu', 4, 1, '1993-04-07', 'Majin Buu Saga', 'Majin Buu'),
    ('The Final Battle', 4, 12, '1993-06-23', 'Majin Buu Saga', 'Majin Buu');


CREATE ROLE goku WITH LOGIN PASSWORD 'goku';
CREATE ROLE vegeta WITH LOGIN PASSWORD 'vegeta';


GRANT SELECT ON character TO goku;
GRANT SELECT ON episode TO vegeta;
```

```sql
CREATE TABLE pokemon (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    type1 VARCHAR(50),
    type2 VARCHAR(50),
    hp INT,
    attack INT,
    defense INT,
    special_attack INT,
    special_defense INT,
    speed INT
);

INSERT INTO pokemon (name, type1, type2, hp, attack, defense, special_attack, special_defense, speed) VALUES
    ('Pikachu', 'Electric', NULL, 35, 55, 40, 50, 50, 90),
    ('Charmander', 'Fire', NULL, 39, 52, 43, 60, 50, 65),
    ('Bulbasaur', 'Grass', 'Poison', 45, 49, 49, 65, 65, 45),
    ('Squirtle', 'Water', NULL, 44, 48, 65, 50, 64, 43),
    ('Jigglypuff', 'Normal', 'Fairy', 115, 45, 20, 45, 25, 20),
    ('Gengar', 'Ghost', 'Poison', 60, 65, 60, 130, 75, 110),
    ('Mewtwo', 'Psychic', NULL, 106, 110, 90, 154, 90, 130),
    ('Snorlax', 'Normal', NULL, 160, 110, 65, 65, 110, 30),
    ('Dragonite', 'Dragon', 'Flying', 91, 134, 95, 100, 100, 80),
    ('Eevee', 'Normal', NULL, 55, 55, 50, 45, 65, 55);

CREATE ROLE ash WITH LOGIN PASSWORD 'ash';
GRANT SELECT ON pokemon TO ash;
```