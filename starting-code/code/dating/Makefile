dating-server:
	$(TROUPE)/bin/troupec dating-server.trp -o ./out/dating-server.js
	node $(TROUPE)/rt/built/troupe.js -f=./out/dating-server.js --id=../ids/id-server.json --trustmap=../trustmaps/GoT.json --aliases=aliases.json

alice-client:
	$(TROUPE)/bin/troupec alice-client.trp -o ./out/alice-client.js
	node $(TROUPE)/rt/built/troupe.js -f=./out/alice-client.js --id=../ids/id-alice.json --trustmap=../trustmaps/client.json

bob-client:
	$(TROUPE)/bin/troupec bob-client.trp -o ./out/bob-client.js
	node $(TROUPE)/rt/built/troupe.js -f=./out/bob-client.js --id=../ids/id-bob.json --trustmap=../trustmaps/client.json

malice-client:
	$(TROUPE)/bin/troupec malice-client.trp -o ./out/malice-client.js
	node $(TROUPE)/rt/built/troupe.js -f=./out/malice-client.js --id=../ids/id-malice.json --trustmap=../trustmaps/client.json