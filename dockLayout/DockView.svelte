<script lang="ts">
    import {onDestroy, onMount} from "svelte";
    import "dockview/dist/styles/dockview.css";
    import {DockviewComponent, Orientation} from "dockview-core";
    import {dockComponents, dockPanels, type Panel} from "@src/store/dockLayoutStore";

    let dockview: DockviewComponent;
    let dockElement: any;
    let parentElement: any;

    export function updateComponents(newComponents: any) {
        dockview.updateOptions({components: newComponents});

        // your store
        dockPanels.subscribe((p: Panel[]) =>
            p.forEach((panel: Panel) => {
                if (!dockview.panels.find((p) => p.id === panel.id)) dockview.addPanel(panel);
            })
        );
    }

    onMount(() => {
        dockview = new DockviewComponent({
            components: {},
            singleTabMode: "default",
            disableFloatingGroups: true,
            parentElement: dockElement,
        });

        parentElement.appendChild(dockElement);

        dockview.onDidRemovePanel(e => {
            if (dockview.panels.find((p) => p.id !== e.id)) {
                dockPanels.update(panels => {
                    return panels.filter((p) => p.id !== e.id);
                })
            }
        });

        dockComponents.subscribe((comp) => updateComponents(comp));
    });

    onDestroy(() => {
        dockview?.dispose();
        dockElement?.remove();
        parentElement?.remove();
    });
</script>

<div bind:this={dockElement} class="dockview-theme-light"></div>
<div bind:this={parentElement}></div>

<!--There you can bind your own components with PanelComponent-->