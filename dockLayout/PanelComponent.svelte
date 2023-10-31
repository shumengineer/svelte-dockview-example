<script lang="ts">
    import type {
        IContentRenderer,
    } from "dockview-core";

    let element;

    // Use this class as DockView Component
    export class CustomPanel implements IContentRenderer {
        private _element: HTMLElement;

        get element(): HTMLElement {
            return this._element;
        }

        constructor() {
            this._element = element;
        }

        init(): void { // params: IGroupPanelInitParameters
            //   this._element.textContent = params.params.title;
        }

        // update(): void { // event: PanelUpdateEvent<Parameters>
        //     //   this._element.textContent = event.params.title;
        // }
    }
</script>

<div bind:this={element}>
    <slot/>
</div>
