<script>
    import { FileList, File } from "svelte-appwrite";
</script>

<h2>Gallery</h2>
<FileList let:files>
    {#each files as file}
        <File {file} let:actions>
            <a href={actions.download()}>
                <img src={actions.preview('300')} alt={file.name} />
            </a>
        </File>
    {/each}
</FileList>