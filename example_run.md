# Example instructions to run GOPHER classifier on MareNostrum

Upload to your preferred working directory the folder "inputs/classifiers" from [GOPHER GitLab repo](https://pm.bsc.es/gitlab/applications/gopher) or the archive "classifers.tar" from [here](https://github.com/cirillodavide/gopher_usecase).

By running the following command, the odds of the association between HP:0001000 and GO:0046623 (noted as p1000 and g46623, i.e. zeros from the unique 7-digit identiers are removed) is computed using human ontologies from 2019:

`/apps/PM/share/gopher/gopher.seq -f /apps/PM/share/gopher/2019G classifiers/2019 -c CLASSIFY p1000:g46623`

Results show the graph loading time and the odds using four different models (called "2019_Gg", "2019_pG", "2019_None", "2019_pGg").

Please refer to the official [execution documentation](https://github.com/cirillodavide/gopher_usecase/blob/master/doc_run.md) for more details and other execution modalities.

Please note that the version of GOPHER installed in MareNostrum only contains the graph composed of human HPO and GO ontologies. However, the version on the GOPHER GitLab repo also contains graphs composed of HPO and DO in mouse as well as GO and DO in fruitfly.
