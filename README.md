var options = {
    tooltips: {
        mode: 'index',
        intersect: false,
        backgroundColor: 'rgba(0, 0, 0, 0.7)',
        titleFontColor: '#fff',
        bodyFontColor: '#fff',
        footerFontColor: '#fff',
        callbacks: {
            label: function(tooltipItem, data) {
                var dataset = data.datasets[tooltipItem.datasetIndex];
                var label = dataset.label || '';
                var value = dataset.data[tooltipItem.index];
                return label + ': ' + value;
            }
        },
        custom: function(tooltipModel) {
            // Tooltip Element
            var tooltipEl = document.getElementById('chartjs-tooltip');

            // Create element on first render
            if (!tooltipEl) {
                tooltipEl = document.createElement('div');
                tooltipEl.id = 'chartjs-tooltip';
                tooltipEl.classList.add('custom-tooltip');
                this._chart.canvas.parentNode.appendChild(tooltipEl);
            }

            // Hide if no tooltip
            if (tooltipModel.opacity === 0) {
                tooltipEl.style.opacity = 0;
                return;
            }

            // Set caret position
            tooltipEl.classList.remove('above', 'below', 'no-transform');
            if (tooltipModel.yAlign) {
                tooltipEl.classList.add(tooltipModel.yAlign);
            } else {
                tooltipEl.classList.add('no-transform');
            }

            // Set Text
            if (tooltipModel.body) {
                var bodyLines = tooltipModel.body.map(function(bodyItem) {
                    return bodyItem.lines;
                });

                var innerHtml = '<div style="text-align: center;">';

                bodyLines.forEach(function(body, i) {
                    var colors = tooltipModel.labelColors[i];
                    var style = 'background:' + colors.backgroundColor;
                    style += '; border-color:' + colors.borderColor;
                    style += '; border-width: 2px';
                    var span = '<span style="' + style + '"></span>';
                    innerHtml += '<div>' + span + body + '</div>';
                });

                innerHtml += '</div>';

                var tooltipRoot = tooltipEl.querySelector('div');
                tooltipRoot.innerHTML = innerHtml;
            }

            // Tooltip Position
            var position = this._chart.canvas.getBoundingClientRect();

            // Display and position tooltip
            tooltipEl.style.opacity = 1;
            tooltipEl.style.position = 'absolute';
            tooltipEl.style.left = position.left + window.pageXOffset + tooltipModel.caretX + 'px';
            tooltipEl.style.top = position.top + window.pageYOffset + tooltipModel.caretY + 'px';
        }
    }
};

var myChart = new Chart(ctx, {
    type: 'bar',
    data: data,
    options: options
});
