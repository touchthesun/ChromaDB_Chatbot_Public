# Metadata function notes

# Article/Topic Category: 
If you want the LLM to determine the category, you could have a conversation where the system role asks the model to categorize the article. This could be done during the conversation where a new article is created or updated.

This should most likely be defined using a system message, such as 'system_kb_metadata.txt', which prompts the LLM to analyze the KB and suggest 1-3 topic/categories in the form of short phrases, not to exceed n chars. These suggestions should be presented to USER for selection, in such a way that if no selection is made, the [0] item is used by default. 

As these metadata will be saved in the kb text files, they can be manually changed or reviewed later.


After the conversation ends and you have the new/updated article text and the article category, you would add this to your metadata. For example:

metadata = {
    'category': category,
}


# TO DO
- Look at plugins / Langchains to integrate necessary visualization tools
- sketch out query and analysis functions and requirements for further elaboration 
- Sketch out what would be required to add external link / document inclusion in KB schema

# Date Created and Modified: 
You would initialize the 'created_at' field when the article is created and then update the 'last_modified_at' field each time the article is updated.

For example:

# When creating a new article:
metadata = {
    'created_at': datetime.now(),
    'last_modified_at': datetime.now(),
    # ... other metadata fields ...
}
collection.add(documents=[article], ids=[new_id], metadata=[metadata])



# Long term plans:
 Use metadata to allow indexing, searching, and visualization of personal KB, in a secure, private and local format. For example, USER should be able to ask the LLM questions about all prior KBs pertaining to a given topic, or use the metadata to visualize connections between KBs using knowledge graphs. 


# When updating an article:
metadata = collection.get_metadata(id=kb_id)[0]
metadata['last_modified_at'] = datetime.now()
collection.update(ids=[kb_id], documents=[article], metadata=[metadata])

Usage Stats: You would increment a 'usage_count' field each time an article is used in a conversation. Depending on your specific needs, you might consider an article as being 'used' when it is included in a response, when it is updated, or at some other point.

For example:

# When an article is used:
metadata = collection.get_metadata(id=kb_id)[0]
metadata['usage_count'] += 1
collection.update(ids=[kb_id], documents=[article], metadata=[metadata])

Please remember to replace the placeholders (like article, new_id, and kb_id) with your actual data. This implementation will also need to be adapted to suit your specific needs and may need additional error checking or functionality.

This new functionality will provide more detailed information about each of your articles, which can help with debugging, analysis, and future development. But remember that metadata is only useful if you use it, so make sure to update your querying and analysis code to make use of these new fields.


