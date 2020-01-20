# HV19.13 TrieMe



**Task:** Switzerland's national security is at risk. As you try to infiltrate a secret spy facility to save the nation you stumble upon an interesting looking login portal. Can you break it and retrieve the critical information?

- Facility: http://whale.hacking-lab.com:8888/trieme/
- [HV19.13-NotesBean.java.zip](https://academy.hacking-lab.com/api/media/challenge/zip/34913db9-fd2a-43c8-b563-55a1d10ee4cb.zip)

**Flag:** `HV19{get_th3_chocolateZ}`

# Research

There is always this one challenge where you continuously bang your head on the keyboard, which means the keyboard is so close you can't read the letters of the solution (Dots, I'm lookin' at you). It took me way too long and many, many hints, but here it is.

The source code shows that a Patricia Trie is used to let users in or not - which is already a strange decision for this kind of task. Also, the differentiation between "admin" and "everyone else" is implemented in a strange way: The code checks if the security token exists in the Patricia Trie. If it *doesn't* exist, you're admin.

```java
private static final String securitytoken = "auth_token_4835989";
// ...
public String getTrie() throws IOException {
	if(isAdmin(trie)) { // returns true if the security token is not in the trie
		InputStream in=getStreamFromResourcesFolder("data/flag.txt");
		StringWriter writer = new StringWriter();
		IOUtils.copy(in, writer, "UTF-8");
		String flag = writer.toString();

		return flag;
	}
	return "INTRUSION WILL BE REPORTED!";
}
// ...
private static boolean isAdmin(PatriciaTrie<Integer> trie){
	return !trie.containsKey(securitytoken);
}
```
It turns out: The implementation of the Patricia Trie in the Apache commons collection contains a bug: The PatriciaTrie ignores trailing null characters in keys.

The bug report: https://issues.apache.org/jira/browse/COLLECTIONS-714

Relevant excerpt:

```java
public void testNullTerminatedKey2() {
    PatriciaTrie<Integer> trie = new PatriciaTrie<>();
    trie.put("x", 0);
    Assert.assertTrue(trie.containsKey("x")); // ok
    trie.put("x\u0000", 1);
    Assert.assertTrue(trie.containsKey("x")); // fail
}
```

"In the above example, the key "x" suddenly disappears when an entry with key "x\u0000" is inserted."

We can now exploit this bug to remove the security token from the Patricia Trie and gain the flag. All we have to do is sending this POST request: `name=auth_token_4835989%5Cu0000` or simply enter `auth_token_4835989\u0000` in the login field.

Let's hope the Swiss military chocolate backup will last beyond Christmas or a national crisis is certain.



