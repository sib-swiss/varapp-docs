
Changelog
=========

16th April 2021

* Docker support
* Added GnomAD frquency filter
* Moved source code to
  https://github.com/sib-swiss/varapp-backend-py,
  https://github.com/sib-swiss/varapp-frontend-react,
  https://github.com/sib-swiss/varapp-docs

16th August 2016
----------------

* BAM server:

  - Reduced to a few lines now that Netty can handle Range requests.
  - Documented in its README.rst.
    
* IGV view of alignments:

  - Now appears in a Modal on double-click on a variant.
  - Documented (see How can I try it, Production setup, User's manual).
  - Sources of IGV.js now bundled with the project, and loaded lazily 
    (new `async` property of the <script> tag).
  - Available on the demo version with de HapMap data (double-click on variant in chr1).
  - Fixed HTTPS configuration.
  - Replaced default gene tracks by public URLs.
    
* Made it visible that the Location filter is computing (loading gif in its area).
* Minified CSS.
* Cron job to dump the prod db once a day.
* Documented how to set up HTTPS (most frequent request from external users).
* Tagged v1.1 .

Major bug fixes

* Fixed BAM server (yet useless) secret key visible in `ps -ef` output.

Minor bug fixes

* Throw an error if a location query is made over more than 300 genes/intervals, 
  instead of failing silently.
* Prevent a user of the demo version to create more than 15 bookmarks.
* Check connection to BAM server with HEAD query (instead of GET).

Functional tests

* Location filter with too many intervals in the query.
* IGV view.



27th June - 8th July 2016
-------------------------

* Bam-server is operational. It serves Igv.js in varapp directly without processing 
  (no need samtools, no intermediate file, no Apache). 
* Deploy script for bam-server, + public config file. Deployed on dev VM.
* Clicking on a variant opens an IGV window showing the alignments for the selected samples 
  (up to 6, and provided the BAM exists and is referenced appropriately in the db).
  The window gets closed when changing a filter, sample, the variants database, or when clicking a Close button.
* Func tests: the new Location text area, new behavior or affected/not samples selection buttons.
* Show the app's version in the footer.
* Added BioRxiv link in the docs under "About Varapp > Reference".
* Removed bootstrap.js dependency (conflicting with jquery-ui required by igv.js); 
  replaced its only occurrence by react-bootstrap's Accordion component (removed 100 lines of ugly code).
* Get igv.js from url, but its dependences from bower.



20th June - 24th June 2016
--------------------------

* Created a simple local BAM server in Scala (separate project).
* Possible to enter a list of genes as a column in a text area (easier to browse than the input field).
* "Affected/not affected" buttons in samples selection tab now deselect the unaffected/affected samples 
  of the selection, respectively (instead of just making them invisible).

Functional tests:

* Demo and prod instances also have func tests running, at least to know the server is up.
* Lookup window

Docs:

* Refactor deployment docs: local installation for both backend and frontend together on 1 page; production setup; dependencies; dev notes.
* Users database schema exposed.

Minor bug fixes

* Fixed Ensembl and Entrez IDs that disappeared from lookup.
* Fixed lookup freezing when selecting a second variant.
* Fixed NaNs in max frequency column.
* Fixed "Back" button disappearing from Admin panel when tables get big. #dirtycss
* Sorted impacts by alphabetical order.
* Fixed Location filter validation state broken after react-bootstrap update.


6th June - 17th June 2016
-------------------------

Simplified the installation: 

* No more backend dependencies to install: download - edit settings - "install" - "runserver".
* Numpy is now automatically installed before the setup script is run to build Cython extensions.
* Rewrote the docs accordingly so that they are much less frightening,
  separating local/test server from production setup with Apache.
* Distributed to PyPi (but installing from source looks easier when configuration is involved).
* Jenkins jobs that try the installation (backend+frontend) from a user perspective after each new build,
  run the local server and check the response.

Docs:

* Description of genetic scenario filters

Others:

* Set up a background task service (celery). Tested on cache loading, but the goal is to
  run vcf2db to replace Gemini (i.e. remove as a user-side dependency).


23rd May - 3rd June 2016
------------------------

* The frontend can be installed with a simple clone and extract of the precompiled package on GitHub.
* External frontend config file for the user to to set e.g. backend URL, https, etc.
* Refactored and commented the gulp pipeline.
* Now always activate every db found in GEMINI_DB_PATH, and deactivate every one that is not found there.
* Build Cython module only if Cython is found, otherwise use the provided precompiled C.
* Deployment script for the frontend.

Minor bug fixes:

* The footer is now always visible - even in 'mobile phone view'.
* Fixed the Location filter search icon (invisible after react-bootstrap update).

Docs:

* Simplified installation procedure. Alternatively, "from source".
* How to use deployment/test servers, to test that it works before configuring Apache etc.



1st May - 20th May 2016
-----------------------

* Management of gemini databases: what happens when some are added, edited, removed during execution
  (no more need to restart the app on change); what is loaded at startup, what happens to the cache.
* Deployment docs according to the feedback of foreign users trying to install it.
* Users of the demo can only log in as demo/demo, can no more create accounts, change password or their profile information.
* Added HapMap example db to the demo.
* Merged the Redis caches into one.
* Use HTTPS conditionally - change in settings.
* Upgrade of Javascript libs; fixed the many subsequent bugs.
* Redaction of a small publication for reference. Benchmarks.
* Read the gene annotation directly in the db (no more gene_detailed cache).
* Tests with WGS, 6mio variants db (scales for some filters, not for others).
* Check that Redis is present on startup, error meaningfully otherwise.
* Check that the users db is present and has tables on startup, error meaningfully otherwise.
* Django tests on features involving the database (rollback after tests are done).

Major bug fixes:

* Loading a too big db caused an error because its size in bits is bigger than INT. Changed to BIGINT.
* Impact has NULL values in the new Gemini.

Minor bug fixes:

* Django, stop logging all db queries in debug mode.



18th April - 29th April 2016
----------------------------

* Varapp is open-source and available on GitHub: https://github.com/varapp . Tagged 1.0.
* Using numpy.packbits to reduce the cache size by 8x.
* New demo db, new demo instance of Varapp on varapp-demo.vital-it.ch, publicly available. Jenkins job to deploy it.
* Using HTTPS for increased security. Only for dev and demo dbs; the latter has a signed certificate. (The other "prod" VM is internal and without a paid certificate would warn the user that the site is potentially dangerous...).
* Using Redis cache. It stores on disk when the app is down, and is independent from Apache processes (i.e. never killed or duplicated). That is a 20x speedup at app startup. Guarantees that requests never time out again.
* Each run of the main benchmark adds a point on a performance evolution graph.
* Abstract for the SIB days.
* Functional test: Location filter

Docs:

* Moved docs to a separate GitHub repo varapp-docs.
* Documented how to generate the users db schema, and provided data dumps to start up easily.
* Moved the docs to be publicly available as well.
* Documented the Redis cache dependency, and how to set it up.
* Link to the docs from the app and from GitHub/Gitlab's readme.

Major bug fixes:

* Fixed stats_service._init_impacts taking most of the app's running time (by writing better SQL statements).
* Fixed broken Location filter (wrong regex for chrom "chrX", among others).
* Fixed REST tests after HTTPS was setup.
* Moved the definition of available databases out of the settings file.
* Removed ModSecurity from demo - for some reason it blocked any request making use of MySQL.
* Prevented users of the demo to change the "guest" account's settings (e.g. password).

Minor bug fixes:

* Fixed Admin panel columns overlapping.
* Reformatted negative frequencies reported in the new Gemini versions when it does not exist (instead of NULL).
* Tagging emails subject correcly with [varapp].



23rd March - 1st April 2016
---------------------------

* Functional tests:
    * continuous sliders
    * reset filters button
    * bookmarks
    * user account panel
    * db change when in /samples
    * annotation columns selection
* Script to warm up cache for all Gemini dbs found in users db
* Full documentation at `<http://varapp.vital-it.ch/docs/>`_
* Link to the docs from app page (in footer)
* Comparative table of existing variant filtering tools vs Varapp
* Thread-safe loc mem cache (instead of global variables)
* Tried DiskCache, Memcached, Redis, Django caches, and various ways of (de-)serializing data (but nothing beats the above for now because of serialization overhead)
* Warm up every cache as many times as there are spawned Apache processes simultaneously (because each process has its own cache). (For now it is ok as we have only 2 procs.)
* [by Sylvain] Script to run the annotation pipeline automatically when VCF files are deposited in a certain folder (cron job).

Major bug fixes:

* Fixed users being able to change other people's password from their account through REST API (!)
* Fixed broken bookmark loading
* Load AdminStore only if accessing Admin page
* Fixed successive similar HXR calls not cancelling the previous ones anymore
* Removed admin JWTs hard-coded in scripts...
* Fixed broken behavior when changing db from /samples
* Fixed changing db saying "unknown samples" in certain circumstances.
* Fixed fill_dbs script to also set DbAccesses to 0 if a VariantDb gets inactive in favor of an updated one.
  Transmit access to the new one instead.

Minor bug fixes:

* Fixed setting ContinuousFilter value to 1 or more printing "<100%" instead of removing the filter.
* Use only one store to record the router query
* Clean up dev db after functional tests



14th march - 18th march 2016
----------------------------

* Made it possible to synchronize database changes across all instances of the app in one command
* Wrote a script to fill the database according to gemini databases detected in the load folder. It checks if the reference already exists and compares the sha1 sum. If it already exists and the hash is the same, marks it as a child and deactivates the parent.
* Functional tests:
  - samples selection
  - db change
  -detect when all server connections (ajax) are closed to trigger some actions, instead of waiting for components to mount

Major bug fixes:

* Fixed a random event of variants not loading, thanks to a big refactoring. (Functional tests help a lot, I am going to finish them).
* Fixed changing the db having random effects when at /samples.

Minor bug fixes:

* Clear the search bar and reset filter buttons when restoring the original samples selection
* Fixed samples summary showing '?' instead of '0' when the count is undefined.



7th March - 11th March 2016
---------------------------

* Selenium* functional test suite: simulation of users interaction with the browser
* Upgraded react-router to 2.0 (`<https://github.com/reactjs/react-router/blob/master/upgrade-guides/v2.0.0.md>`_)
* Models: link bookmarks to `db_accesses` instead of `users` + `variants_db`. Removed reference to `variants_db` from `history` table.
* Updated test db to include chrX genes and new compound candidates after the filter changed
* Documentation: app deployment, users guide

Major bug fixes:

* Fixed variants not loading when stores are ready but session expired
* Fixed wrong auto redirection to /login on pages that do not require authentication
* Fixed pure-render-mixin causing bugs in data tables
* Stop loading gifs in an error is encountered

Minor bug fixes:

* Handle wrong inputs in continuous filters custom text fields
* Fixed dbsnp ids appearing as lists in VCF output
* Replaced variants count '?' by '...' when stats are loading
* Fixed Reset button not working anymore un UserAccount panel

\* PhantomJS does not work with React. CasperJS uses PhantomJS. Selenium's PhantomJS webdriver uses PhantomJS. Nightwatch uses Selenium with PhantomJS. HTMLUnit ghostdriver is only available in Java. In the end only the Python bindings for selenium are working.



13th February - 19th February 2016
----------------------------------

* Create one random salt per user, store it in database together with hashed password (instead of using a single common salt stored in config file).
* Can select samples in the table by clicking on them in the variants table, and there is a button to move the selection to the top of the table.
* The columns selection is not tied to the db anymore, i.e. one can change the db without losing one's preferences.
* Added a filter on the max frequency of a variant over 1000genomes, ESP and Exac, over all subpopulations (``max_aaf_all`` in gemini schema).
* Tried to get rid of global varianbles for thread-safe caching:
    - Tried Django caches - unusable because it compresses data before storing, thus is very slow (30s to respond).
    - Tried Memcached - unusable because limited to 1MB, and not performing well if set to a higher limit.
* Made cached arrays immutable.
* Impact categories are inconsistent between Ensembl predictions, Gemini docs, and Gemini db... Made at least the app's view consistent with the current database content.
* Speed up of compound het filter in case of many members of the same family.
* Added Gemini version to 'report' export.
* Carefully tested ``extract_variants_from_ids_set``, a core loop that extracts variants from database based on a set of ids.
* Added an Annotations table in users db to record versions of tools and databases used to produce a given gemini database.
    - Created a script to fill in the 'Annotations' table from a gemini db.
* Added a Preferences table; migrated bookmarks from History to a new Bookmarks table. Keep History to record user actions continuously.
* Set up the Django migrations framework - the local users db schema mirrors changes in the python models; SQL commands to redo the changes are generated and can be applied to dev and prod dbs.

Major bug fixes:

* Fixed a case of false positive compound het (discovered by Lucie Gueneau).
* Allow to create a bookmark on first load (when url is empty of parameters after #).

Minor bug fixes:

* Fixed adding/removing a sample displaying a '?' in variants summary.
* Fixed selecting 0 samples displaying a '?' in variants summary.
* Fixed "Potentially unhandled rejection" issued by when.js when auth token expires.
* Fixed updating a sample not updating the URL.



5th February - 12th February 2016
---------------------------------

* Managed to trigger a file download directly from an Ajax call, which allows the next point:
* Protect the export of variants with JWT as well.
* Reworked forms (login, signup etc.)
    - Reusable common components for maintainability
    - Colors, error messages when something is missing, etc.
    - Check format of emails/phone numbers/escape HTML in text fields to protect from XSS attacks.
* Use the same "XHR in actions, not in stores" pattern for login stuff.
* Added link to OMIM from gene lookup.
* Added link to EXAC from exac frequencies column.
* "Back" button from samples selection.
* Signal when we are exporting variants (replaces the button by a progress bar).
* Loading a bookmark no longer reloads the stores (stats, samples, bookmarks etc.).
* Added a "no value" option for selecting variants with NULL values in a given enum field (polyphen/sift pred).
* The selected variant is highlighted.
* The genotypes lookup now shows the parents of each sample, or if it is the mother/father of a family.
* The name of the first/only selected family shows up in the samples summary.
* Colored impacts according to HIGH/MED/LOW categories.
* Added Contact link.
* Upgraded lodash to v4 (breaking API).

Major bug fixes:

* Save state change from samples selection (before, returning from variants selection would leave an empty URL).
* Fixed problems with stores reloading twice at startup.
* Fixed account management fields not to reflect database info correctly after a change.

Minor bug fixes:

* Fixed reloading the page after variant lookup throwing an error.
* Catch "SMTP server not found".
* Fixed wrong number of variants in the report export.
* Fixed broken filter removal from filter group summary.
* The new version is tagged 0.5 and is online on both prod and CHUV VMs.



25th January - 4th February 2016
--------------------------------

* X-linked genotypes filter done.
* Tables now have their dimensions fitting the screen height.
* Reworked the samples selection table. It is now on a separate "page" instead of an openable panel. It shows a summary of the filtered variants, and the variants page show a summary of the samples selection.
    - Having 2 pages required to change how the router handles components, since the two have to stay in sync.
* Reworked the Flux, i.e. how actions are triggered and listened by components. This important refactoring has a lot of beneficial  consequences, among which :
    - Improved stability and maintainability;
    - Signal when async actions start *and* finish.
* On the previous point, implemented components showing that a frame is loading (e.g. loading the next batch of variants when scrolling down) - to replace the older, not visible enough bottom loading gif.
* New button to generate a text report/summary (program versions, samples selection, chosen filters).
* Split the CSS, one sheet per component.
* Show the family name in samples summary, if one is selected.

Major bug fixes:

* Fixed selecting a sample returning back to the first table row.
* Fixed tables sometimes freezing after scroll (infinite loop).
* Fixed stats still reflecting singletons from a compound of which a component got filtered out.
* Fixed wrong sorting of variants after january's work.
* Update the URL when returning from samples selection.

Minor bug fixes:

* Fixed searching for an inexistent gene returning an error.
* Fixed empty string in continuous value filter returning NaN error.
* Check format of search string in Location filter.
