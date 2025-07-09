CREATE TABLE "User" (
  "user_id" varchar(20) PRIMARY KEY,
  "password" varchar(20) NOT NULL,
  "first_name" varchar(20) NOT NULL,
  "middle_name" varchar(20) NOT NULL,
  "last_name" varchar(20) NOT NULL,
   "pincode" integer NOT NULL
);

CREATE TABLE "User_Mobile" (
  "user_id" varchar(20),
  "mobile_no" bigint,
  PRIMARY KEY ("user_id", "mobile_no")
);

CREATE TABLE "Learning_License" (
  "ll_id" varchar(20) PRIMARY KEY,
  "pincode" integer NOT NULL,
  "email" varchar(20) NOT NULL,
  "gender" char NOT NULL,
  "qualification" varchar(20),
  "applicant_category" varchar(25),
  "vehicle_category" varchar(30) NOT NULL,
  "DOB" date NOT NULL,
  "address_proof" bytea NOT NULL,
  "medical_certificate" bytea NOT NULL,
  "id_proof" bytea UNIQUE NOT NULL,
  "applicant_photo" bytea NOT NULL,
  "user_id" varchar(20) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Driving_License" (
  "dl_no" varchar(20) PRIMARY KEY,
  "vehicle_category" varchar(30) NOT NULL,
  "address_proof" bytea NOT NULL,
  "medical_certificate" bytea NOT NULL,
  "id_proof" bytea UNIQUE NOT NULL,
  "applicant_photo" bytea NOT NULL,
  "ll_photo" bytea NOT NULL,
  "ll_id" varchar(20) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Payment" (
  "transaction_id" varchar(20) PRIMARY KEY,
  "payment_mode" varchar(10) NOT NULL,
  "timestamp" TIMESTAMP NOT NULL
);

CREATE TABLE "Renewal" (
  "renewal_id" varchar(20) PRIMARY KEY,
  "dl_no" VARCHAR(15) NOT NULL,
  "renew_date" DATE NOT NULL,
  "id_proof" BYTEA NOT NULL,
  "photo" BYTEA NOT NULL,
  "old_dl" BYTEA NOT NULL,
  "address_proof" BYTEA NOT NULL,
  "med_certificate" BYTEA NOT NULL,
  "user_id" varchar(20) NOT NULL,
  "renew_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Renewal_Payment" (
  "renewal_txn_id" varchar(20) PRIMARY KEY,
  "payment_mode" VARCHAR(20) NOT NULL,
  "timestamp" TIMESTAMPTZ NOT NULL,
  "app_no" varchar(20)
);

CREATE TABLE "Vehicle_Registration" (
  "registration_no" VARCHAR(20) PRIMARY KEY,
  "registration_date" DATE NOT NULL,
  "vehicle_type" VARCHAR(50) NOT NULL,
  "model" VARCHAR(100) NOT NULL,
  "year_of_manufacture" INT NOT NULL,
  "engine_no" VARCHAR(50) NOT NULL,
  "chassis_no" VARCHAR(50) NOT NULL,
  "color" VARCHAR(50) NOT NULL,
  "temp_reg_no" VARCHAR(20),
  "user_id" varchar(20) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL,
   "pincode" integer NOT NULL
);

CREATE TABLE "Vehicle_Owner" (
  "owner_name" VARCHAR(100) NOT NULL,
  "registration_no" VARCHAR(20) NOT NULL,
  PRIMARY KEY ("owner_name", "registration_no")
);

CREATE TABLE "Permit" (
  "permit_no" varchar(20) PRIMARY KEY,
  "route_info" VARCHAR(40) NOT NULL,
  "identity_proof" bytea NOT NULL,
  "purpose" varchar(20) NOT NULL,
  "validity" date NOT NULL,
  "insurance_no" varchar(20) NOT NULL,
  "vehicle_fitness" bytea NOT NULL,
  "user_id" varchar(20) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Appointment" (
  "appointment_no" varchar(20) PRIMARY KEY,
  "date" DATE NOT NULL,
  "status" varchar(15) NOT NULL,
  "applying_for" varchar(20) NOT NULL,
  "branch" varchar(30) NOT NULL,
  "timeslot" timestamp NOT NULL,
  "counter_no" varchar(5) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Test" (
  "roll_no" varchar(20) PRIMARY KEY,
  "test_score" integer NOT NULL,
  "invigilator" varchar(15) NOT NULL,
  "feedback" varchar(30),
  "date" DATE NOT NULL,
  "time_slot" time NOT NULL,
  "computer_no" varchar(5) NOT NULL,
  "app_txn_id" varchar(20) NOT NULL
);

CREATE TABLE "Applied_To" (
  "appointment_no" varchar(20),
  "roll_no" varchar(20),
  "city_code" varchar(10),
  "state_code" varchar(10),
  "counter_no" varchar(5),
	PRIMARY KEY ("appointment_no", "roll_no", "city_code", "state_code", "counter_no")
);

CREATE TABLE "RTO" (
  "city_code" varchar(10),
  "state_code" varchar(10),
  "pincode" integer NOT NULL,
  "number_of_employees" integer NOT NULL,
  "roll_no" varchar(20) NOT NULL,
  "counter_no" varchar(5) NOT NULL,
  PRIMARY KEY ("city_code", "state_code")
);

CREATE TABLE "Counter" (
  "counter_no" varchar(5) PRIMARY KEY,
  "counter_type" varchar(5) NOT NULL,
  "employee_id" integer NOT NULL,
  "employee_name" varchar(20) NOT NULL,
  "roll_no" varchar(20) NOT NULL
);

CREATE TABLE "Pincode" (
  "pincode" integer PRIMARY KEY,
  "state_code" varchar(20) NOT NULL,
  "city" varchar(20) NOT NULL
);

--user reference

ALTER TABLE "User_Mobile" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("user_id");

ALTER TABLE "Learning_License" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("user_id");

ALTER TABLE "Renewal" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("user_id");

ALTER TABLE "Permit" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("user_id");

ALTER TABLE "Vehicle_Registration" ADD FOREIGN KEY ("user_id") REFERENCES "User" ("user_id");

--appointment reference

ALTER TABLE "Renewal_Payment" ADD FOREIGN KEY ("app_no") REFERENCES "Appointment" ("appointment_no");



ALTER TABLE "Applied_To" ADD FOREIGN KEY ("appointment_no") REFERENCES "Appointment" ("appointment_no");

--payment reference

ALTER TABLE "Learning_License" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

ALTER TABLE "Vehicle_Registration" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

ALTER TABLE "Test" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

ALTER TABLE "Permit" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

ALTER TABLE "Appointment" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

ALTER TABLE "Driving_License" ADD FOREIGN KEY ("app_txn_id") REFERENCES "Payment" ("transaction_id");

--pincode reference

ALTER TABLE "Learning_License" ADD FOREIGN KEY ("pincode") REFERENCES "Pincode" ("pincode");

ALTER TABLE "RTO" ADD FOREIGN KEY ("pincode") REFERENCES "Pincode" ("pincode");


--Test reference

ALTER TABLE "RTO" ADD FOREIGN KEY ("roll_no") REFERENCES "Test" ("roll_no");

ALTER TABLE "Applied_To" ADD FOREIGN KEY ("roll_no") REFERENCES "Test" ("roll_no");


--counter reference

ALTER TABLE "Applied_To" ADD FOREIGN KEY ("counter_no") REFERENCES "Counter" ("counter_no");

ALTER TABLE "Test" ADD FOREIGN KEY ("computer_no") REFERENCES "Counter" ("counter_no");

ALTER TABLE "Appointment" ADD FOREIGN KEY ("counter_no") REFERENCES "Counter" ("counter_no");

ALTER TABLE "RTO" ADD FOREIGN KEY ("counter_no") REFERENCES "Counter" ("counter_no");


--other references

ALTER TABLE "Applied_To" ADD FOREIGN KEY ("city_code", "state_code") REFERENCES "RTO" ("city_code", "state_code");

ALTER TABLE "Driving_License" ADD FOREIGN KEY ("ll_id") REFERENCES "Learning_License" ("ll_id");

ALTER TABLE "Renewal" ADD FOREIGN KEY ("renew_txn_id") REFERENCES "Renewal_Payment" ("renewal_txn_id");

ALTER TABLE "Vehicle_Owner" ADD FOREIGN KEY ("registration_no") REFERENCES "Vehicle_Registration" ("registration_no");