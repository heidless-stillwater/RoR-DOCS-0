

UPDATE users 
SET confirmed_at = NOW();


DELETE FROM users where ID = 2;

SELECT * FROM users;