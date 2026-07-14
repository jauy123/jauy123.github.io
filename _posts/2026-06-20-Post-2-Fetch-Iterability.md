## Post 2 - On the interoperability of the Fetch module with other MDAnalysis Classes.

An interesting question that came up during the development of the `fetch` module is whether it should interact with other parts of the `MDAnalysis` codebase. For example, one could imagine a method within `Universe`, such as`Universe.from_PDB()`, or an argument to the `Universe` constructor, such as `Universe(pdb_code)`, that calls fetch and initializes the `Universe` directly.

While this sounds useful at first glance, I would personally disagree with adding this functionality to the main `Universe` or modifying already existing `MDAnalysis` objects for the following reasons:

1.	The fetch module should follow KISS principles — Keep It Simple, Stupid — and remain independent of the existing code.

2.	Downloading files is not essential to the main functionality of `MDAnalysis`.

3.	Modifying core `MDAnalysis` objects would add unnecessary technical burden to the main codebase.

The primary purpose of `MDAnalysis` is to provide a user-friendly framework for analyzing molecular dynamics trajectories. Therefore, downloading files should remain secondary to analyzing physical files that already exist on disk. This is especially true because most trajectories are produced by research groups and typically live locally unless they are published to a database (and even that isn’t guaranteed if the trajectory isn’t interesting enough – looking at you Alanine Dipeptide).

Additionally, the current implementation of the fetch module uses `pooch` as an optional dependency. Making pooch a required dependency of the main codebase feels excessive and would not be worth it from a maintenance perspective. This is true especially considering the prior point of most trajectories living on disk. Practically, this means that the fetch module should remain independent from the main codebase, be optional to use, and ideally follow KISS principles.

KISS is a methodology that encourages code to remain “simple.” While “simple” is not a precisely defined term, I interpret it here as following a straightforward framework. There are two main classes that users should care about: `StaticFetcher()`, which downloads files to disk and returns a `pathlib.Path` object readable by the core Universe object, and `DynamicFetcher()`, which yields a generator that can be iterated over during analysis. Convenience functions such as `from_pdb()` can call these classes in predetermined way, but users can still use the downloaders classes in more sophisticated workflows if they choose.

This is best emphasized in the current iteration of `from_pdb` at the time of writing. It just purely does preprocessing to `StaticFetcher` which downloads to `cache_path`. There’s nothing stopping the user from writing the same function themselves which is why they are considered convivence functions.

```python
if file_format not in SUPPORTED_FILE_FORMATS_DOWNLOADER:
        raise ValueError(
            "Invalid file format. Supported file formats "
            f"are {SUPPORTED_FILE_FORMATS_DOWNLOADER}"
        )

    pdb_ids = [pdb + "." + file_format for pdb in pdb_ids]

    fetcher = StaticFetcher(cache_path=cache_path)
    return fetcher.fetch(
        file_name=pdb_ids,
        base_url="https://files.wwpdb.org/download/",
        progressbar=progressbar,
    )
```



